---
title: "RabbitMQ高级"
date: 2024-07-27
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Bimage-135/Bimage84.jpg?raw=true
tags: ["SpringCloud","RabbitMQ"]
category: "微服务"
updated: 2024-07-28
  
top_group_index: 
---

# 消息的可靠性

![从生产者发送消息到消费者处理消息](../images/从生产者发送消息到消费者处理消息.png)

消息从生产者到消费者的每一步都可能导致消息丢失:

**发送消息时丢失**:
- 生产者发送消息时连接MQ失败
- 生产者发送消息到达MQ后未找到Exchange
- 生产者发送消息到达MQ的Exchange后,未找到合适的Queue
- 消息到达MQ后,处理消息的进程发生异常

**MQ导致消息丢失**:
- 消息到达MQ,保存到队列后,尚未消费就突然宕机

**消费者处理消息时**:
- 消息接收后尚未处理突然宕机
- 消息接收后处理过程中抛出异常

综上,要解决消息丢失问题,保证MQ的可靠性,必须从3个方面入手:
- 确保生产者一定把消息发送到MQ(生产者的可靠性)
- 确保MQ不会将消息弄丢(MQ的可靠性)
- 确保消费者一定要处理消息(消费者的可靠性)

# 生产者的可靠性

## 生产者重连机制

有的时候由于网络波动,可能会出现生产者连接MQ失败的情况

通过配置可以开启连接失败后的重连机制:

```yaml
spring:
  rabbitmq:
    connection-timeout: 1s # 设置MQ的连接超时时间
    template:
      retry:
        enabled: true # 开启超时重连机制
        initial-interval: 1000ms # 失败后的初始等待时间
        multiplier: 1 # 失败后下次的等待时长倍数,下次等待时长 = initial-interval * multiplier
        max-attempts: 3 # 最大重连次数
```

当网络不稳定的时候,利用重试机制可以有效提高消息发送的成功率,不过SpringAMQP提供的重试机制是**阻塞式**的重试,也就是说多次重试等待的过程中,当前线程是被阻塞的,会影响业务性能,如果对于业务性能有要求,**建议禁用重试机制**,如果一定要使用,请合理配置等待时长和重试次数,当然也**可以考虑使用异步线程来执行发送消息的代码**

## 生产者确认机制

SpringAMQP提供了**Publisher Confirm**和**Publisher Return**两种确认机制

开启确认机制后,当生产者发送消息给MQ后,MQ会返回确认结果给生产者,返回的结果有以下几种情况:
- 消息投递到了MQ,但是路由失败,此时会通过PublisherReturn返回路由异常原因,然后返回**ACK**,告知投递成功
- 临时消息投递到了MQ,并且入队成功,返回**ACK**,告知投递成功
- 持久消息投递到了MQ,并且入队完成持久化,返回**ACK**,告知投递成功
- 其它情况都会返回**NACK**,告知投递失败

![生产者确认](../images/生产者确认.png)

如何处理生产者的确认消息:
- 生产者确认需要额外的网络和系统资源开销,尽量不要使用
- 对于nack消息可以有限次数重试,依然失败则记录异常消息

开启生产者确认比较消耗MQ性能,**一般不建议开启**

触发确认的几种情况:
- 路由失败:一般是因为RoutingKey错误导致,往往是编程导致
- 交换机名称错误:同样是编程错误导致
- MQ内部故障:这种需要处理,但概率往往较低,因此只有对消息可靠性要求非常高的业务才需要开启,而且仅仅需要开启ConfirmCallback处理nack就可以

### 实现生产者确认

1. 在publisher这个微服务的application.yml中添加配置

```yaml
spring:
  rabbitmq:
    publisher-confirm-type: correlated # 开启publisher confirm机制,并设置confirm类型
    publisher-returns: true # 开启publisher return机制
```

配置说明:
这里publisher-confirm-type有三种模式可选:
- `none`:关闭confirm机制(默认)
- `simple`:同步阻塞等待MQ的回执消息
- `correlated`:MQ异步回调方式返回回执消息

2. 每个RabbitTemplate**只能配置一个**ReturnCallback,因此需要在项目启动过程中配置

```java
import lombok.AllArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.ReturnedMessage;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.context.annotation.Configuration;

import javax.annotation.PostConstruct;

@Slf4j
@AllArgsConstructor
@Configuration
public class MqConfig {
    private final RabbitTemplate rabbitTemplate;

    @PostConstruct
    public void init(){
        rabbitTemplate.setReturnsCallback(new RabbitTemplate.ReturnsCallback() {
            @Override
            public void returnedMessage(ReturnedMessage returned) {
                log.error("触发return callback,");
                log.debug("exchange: {}", returned.getExchange());
                log.debug("routingKey: {}", returned.getRoutingKey());
                log.debug("message: {}", returned.getMessage());
                log.debug("replyCode: {}", returned.getReplyCode());
                log.debug("replyText: {}", returned.getReplyText());
            }
        });
    }
}
```

3. 发送消息,指定消息ID、消息ConfirmCallback

由于每个消息发送时的处理逻辑不一定相同,因此ConfirmCallback需要在每次发消息时定义

具体来说,是在调用RabbitTemplate中的convertAndSend方法时,多传递一个参数:CorrelationData

CorrelationData中包含两个核心的东西:
- `id`:消息的唯一标示,MQ对不同的消息的回执以此做判断,避免混淆
- `SettableListenableFuture`:回执结果的Future对象

将来MQ的回执就会通过这个Future来返回,可以提前给CorrelationData中的Future添加回调函数来处理消息回执

```java
@Test
void testPublisherConfirm() {
    // 1.创建CorrelationData
    CorrelationData cd = new CorrelationData();
    // 2.给Future添加ConfirmCallback
    cd.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
        @Override
        public void onFailure(Throwable ex) {
            // 2.1.Future发生异常时的处理逻辑,基本不会触发
            log.error("send message fail", ex);
        }
        @Override
        public void onSuccess(CorrelationData.Confirm result) {
            // 2.2.Future接收到回执的处理逻辑,参数中的result就是回执内容
            if(result.isAck()){ // result.isAck(),boolean类型,true代表ack回执,false代表nack回执
                log.debug("发送消息成功,收到 ack!");
            }else{ // result.getReason(),String类型,返回nack时的异常描述
                log.error("发送消息失败,收到 nack, reason : {}", result.getReason());
            }
        }
    });
    // 3.发送消息
    rabbitTemplate.convertAndSend("hmall.direct", "red", "hello", cd);
}
```

### 范例

publisher的配置文件:

```yaml
logging:
  pattern:
    dateformat: MM-dd HH:mm:ss:SSS
spring:
  rabbitmq:
    host: 192.168.149.127 # 主机名
    port: 5672
    virtual-host: /hmall
    username: hmall
    password: 123456
    listener:
      simple:
        prefetch: 1 # 每次只能获取一条消息,处理完成才能获取下一个消息
    template:
      retry:
        enabled: true # 开启超时重试机制
        initial-interval: 1000ms # 失败后的初始等待时间
        multiplier: 2 # 失败后下次的等待时长倍数,下次等待时长 = initial-interval * multiplier
        max-attempts: 3 # 最大重试次数
    publisher-confirm-type: correlated # 开启publisher confirm机制,并设置confirm类型
    publisher-returns: true # 开启publisher return机制
```

定义ReturnCallback:

```java
package com.itheima.publisher.config;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.context.annotation.Configuration;

import javax.annotation.PostConstruct;

@Slf4j
@Configuration
@RequiredArgsConstructor
public class MqConfig {
    private final RabbitTemplate rabbitTemplate;

    @PostConstruct
    public void init() {
        rabbitTemplate.setReturnsCallback(returned -> {
            log.error("监听到了消息:return callback,");
            log.debug("exchange: {}", returned.getExchange());
            log.debug("routingKey: {}", returned.getRoutingKey());
            log.debug("message: {}", returned.getMessage());
            log.debug("replyCode: {}", returned.getReplyCode());
            log.debug("replyText: {}", returned.getReplyText());
        });
    }
}
```

创建correlationData:

```java
@Test
public void testConfirmCallback() {
    // 创建CorrelationData
    CorrelationData cd = new CorrelationData();
    // 给Future添加ConfirmCallback
    cd.getFuture().addCallback(new ListenableFutureCallback<CorrelationData.Confirm>() {
        @Override
        public void onFailure(Throwable ex) {
            // Future发生异常时的处理逻辑,基本不会触发
            log.error("SpringAMQP处理确认结果异常", ex);
        }

        @Override
        public void onSuccess(CorrelationData.Confirm result) {
            // 判断是否成功,true代表ack回执,false代表nack回执
            if (result.isAck()) {
                log.debug("发送消息成功,收到 ack!");
            } else {
                log.error("发送消息失败,收到 nack, 失败原因 : {}", result.getReason());
            }
        }
    });
    // 交换机名称
    String exchangeName = "object.topic";
    // 消息
    Map<String, Object> map = new HashMap<>(2);
    map.put("name", "张三");
    map.put("age", 18);
    // 发送消息
    rabbitTemplate.convertAndSend(exchangeName, "red", map, cd);
}
```

# MQ的可靠性

在默认情况下,RabbitMQ会将接收到的信息保存在内存中以降低消息收发的延迟,这样会导致两个问题:
- 一旦MQ宕机,内存中的消息会丢失
- 内存空间有限,当消费者故障或处理过慢时,会导致消息积压,引发MQ阻塞

## 数据持久化

RabbitMQ实现数据持久化包括3个方面:

- 交换机持久化(添加时默认Durable)

![交换机持久化](../images/交换机持久化.png)

- 队列持久化(添加时默认Durable)

![队列持久化](../images/队列持久化.png)

- 消息持久化

![消息持久化](../images/消息持久化.png)

细节:
1. **SpringAMQP中的交换机、队列、消息默认持久化**
2. **开启持久化和发送者确认时,RabbitMQ只有在消息持久化完成后才会给发送者返回ACK回执**

## 惰性队列

从RabbitMQ的3.6.0版本开始,就增加了Lazy Queue的概念,也就是**惰性队列**

惰性队列的特征如下:
- 接收到消息后直接存入磁盘,不再存储到内存
- 消费者要消费消息时才会从磁盘中读取并加载到内存(可以提前缓存部分消息到内存,最多2048条)

**在RabbitMQ的3.12版本后,所有队列都是Lazy Queue模式,无法更改**

### 实现

1. 控制台设置Lazy Queue

![控制台设置Lazy Queue](../images/Lazy%20Queue.png)

2. 代码实现Lazy Queue

```java
@Bean
public Queue lazyQueue(){
    return QueueBuilder            
            .durable("lazy.queue")
            .lazy() // 开启Lazy模式
            .build();
}
```

```java
@RabbitListener(bindings = @QueueBinding(
        value = @Queue(name = "topic.queue", durable = "true", arguments = @Argument(name = "x-queue-mode", value = "lazy")),
        exchange = @Exchange(name = "topic.exchange", type = ExchangeTypes.TOPIC),
        key = {"red", "blue"}))
public void listenTopicQueue(String msg) {
    log.info("消费者接收到了消息" + msg);
}
```

# 消费者的可靠性

## 消费者确认机制

消费者确认机制(Consumer Acknowledgement)是为了确认消费者是否成功处理消息

当消费者处理消息结束后,应该向RabbitMQ发送一个回执,告知RabbitMQ自己消息处理状态:
- `ack`:成功处理消息,RabbitMQ从队列中删除该消息
- `nack`:消息处理失败,RabbitMQ需要再次投递消息
- `reject`:消息处理失败并拒绝该消息,RabbitMQ从队列中删除该消息

![消费者确认机制](../images/消费者确认机制.png)

SpringAMQP已经实现了消息确认功能,并允许通过配置文件选择ACK处理方式,有三种方式:
- `none`:不处理,即消息投递给消费者后立刻ack,消息会立刻从MQ删除。非常不安全,不建议使用
- `manual`:手动模式,需要自己在业务代码中调用api,发送ack或reject,存在业务入侵,但更灵活
- `auto`:自动模式,SpringAMQP利用AOP对消息处理逻辑做了环绕增强,当业务正常执行时则自动返回ack,当业务出现异常时,根据异常判断返回不同结果:
    - 如果是业务异常,会自动返回nack
    - 如果是消息处理或校验异常,自动返回reject

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        acknowledge-mode: auto # none,关闭ack;manual,手动ack;auto,自动ack
```

## 消费者失败重试机制

SpringAMQP提供了消费者失败重试机制,在消费者出现异常时利用本地重试,而不是无限的requeue到mq

可以通过配置文件来开启重试机制:

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true # 开启消费者失败重试机制
          initial-interval: 1000ms # 失败后的初始等待时间
          multiplier: 2 # 失败后下次的等待时长倍数,下次等待时长 = initial-interval * multiplier
          max-attempts: 3 # 最大重试次数
          stateless: true # true无状态;false有状态,如果业务中包含事务,这里改为false
```

## 业务幂等性


# 延迟消息