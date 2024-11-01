---
title: "Spring面试题"
date: 2024-09-10
description: ""
cover: https://github.com/Gjt-9520/MarkDownBlog/blob/main/source/coverImages/Bimage-135/Bimage126.jpg?raw=true
tags: ["Spring"]
category: "面试"
updated: 2024-09-11
  
top_group_index: 
---

# Spring用到了哪些设计模式？

1. 简单工厂模式：BeanFactory就是简单工厂模式的体现，根据传入一个唯一标识来获得Bean对象。

2. 工厂方法模式：FactoryBean就是典型的工厂方法模式。spring在使用getBean()调用获得该Bean时，会自动调用该Bean的getObject()方法。每个都会Bean对应一个FactoryBean，如SqlSessionFactory对应SqlSessionFactoryBean。

3. 单例模式：一个类仅有一个实例，提供一个访问它的全局访问点。Spring创建Bean实例默认是单例的。

4. 适配器模式：SpringMVC中的适配器HandlerAdatper。由于应用会有多个Controller实现，如果需要直接调用Controller方法，那么需要先判断是由哪一个Controller处理请求，然后调用相应的方法。当增加新的Controller，需要修改原来的逻辑，违反了开闭原则（对修改关闭，对扩展开放）。

5. 代理模式：Spring的aop使用了动态代理，有两种方式JdkDynamicAopProxy和Cglib2AopProxy。

6. 观察者模式：Spring中observer模式常用的地方是listener的实现，如ApplicationListener。

7. 模板模式：Spring中jdbcTemplate、hibernateTemplate等，就使用到了模板模式。

# AOP有哪些实现方式？

AOP有两种实现方式：静态代理和动态代理。

1. 静态代理

代理类在编译阶段生成，在编译阶段将通知织入Java字节码中，也称编译时增强。AspectJ使用的是静态代理。

缺点：代理对象需要与目标对象实现一样的接口，并且实现接口的方法，会有冗余代码。同时，一旦接口增加方法，目标对象与代理对象都要维护。

2. 动态代理

代理类在程序运行时创建，AOP框架不会去修改字节码，而是在内存中临时生成一个代理对象，在运行期间对业务方法进行增强，不会生成新类。

# JDK动态代理和CGLIB动态代理的区别？

- JDK动态代理

如果目标类实现了接口，Spring AOP会选择使用JDK动态代理目标类。

代理类根据目标类实现的接口动态生成，不需要自己编写，生成的动态代理类和目标类都实现相同的接口。

JDK动态代理的核心是InvocationHandler接口和Proxy类。

缺点：目标类必须有实现的接口。如果某个类没有实现接口，那么这个类就不能用JDK动态代理。

JDK动态代理范例：

```java
package com.gujintao.ProxyExample.JDK;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理类的接口
 * @author: gujintao
 * @create: 2024-10-25 23:09
 **/
public interface GreetingService {
    void greet(String name);
}
```

```java
package com.gujintao.ProxyExample.JDK;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理类
 * @author: gujintao
 * @create: 2024-10-25 23:10
 **/
public class GreetingServiceImpl implements GreetingService {
    @Override
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
}
```

1. 创建InvocationHandler：实现InvocationHandler接口，定义如何处理代理对象上的方法调用

```java
package com.gujintao.ProxyExample.JDK;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：实现了InvocationHandler接口的切面类
 * @author: gujintao
 * @create: 2024-10-25 23:12
 */
public class LoggingInvocationHandler implements InvocationHandler {
    // 真实的服务对象
    private final Object target;

    public LoggingInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 在方法调用前打印日志
        System.out.println("Before method: " + method.getName());

        // 调用真实的方法
        Object result = method.invoke(target, args);

        // 在方法调用后打印日志
        System.out.println("After method: " + method.getName());

        return result;
    }
}
```

2. 创建代理对象并使用：使用Proxy.newProxyInstance方法创建代理对象，传入类加载器、接口数组和InvocationHandler

```java
package com.gujintao.ProxyExample.JDK;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

/**
 * @program: MvcProject
 * @description: 测试JDK动态代理：代理对象
 * @author: gujintao
 * @create: 2024-10-25 23:13
 **/
public class ProxyExample {
    public static void main(String[] args) {
        // 创建真实的服务对象
        GreetingService realService = new GreetingServiceImpl();

        // 创建InvocationHandler
        InvocationHandler handler = new LoggingInvocationHandler(realService);

        // 创建代理对象
        GreetingService proxyService = (GreetingService) Proxy.newProxyInstance(
                // 类加载器
                GreetingService.class.getClassLoader(),
                // 接口数组
                new Class<?>[]{GreetingService.class},
                // 处理器
                handler
        );

        // 通过代理对象调用方法
        proxyService.greet("World");
    }
}
```

- CGLIB动态代理

通过继承实现。如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。

CGLIB（Code Generation Library）可以在运行时动态生成类的字节码，动态创建目标类的子类对象，在子类对象中增强目标类。

CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

优点：目标类不需要实现特定的接口，更加灵活。

CGLIB动态代理范例：

```java
package com.gujintao.ProxyExample.CGLIB;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：代理类
 * @author: gujintao
 * @create: 2024-10-25 23:21
 **/
public class GreetingService {
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
}
```

1. 创建MethodInterceptor：实现MethodInterceptor接口，定义如何处理代理对象上的方法调用

```java
package com.gujintao.ProxyExample.CGLIB;


import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：实现了MethodInterceptor接口的切面类
 * @author: gujintao
 * @create: 2024-10-25 23:24
 **/
public class LoggingMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        // 在方法调用前打印日志
        System.out.println("Before method: " + method.getName());

        // 调用真实的方法
        Object result = methodProxy.invokeSuper(obj, args);

        // 在方法调用后打印日志
        System.out.println("After method: " + method.getName());

        return result;
    }
}
```

2. 创建代理对象并使用：使用enhancer.create()方法创建代理对象，传入代理类和MethodInterceptor

```java
package com.gujintao.ProxyExample.CGLIB;

import net.sf.cglib.proxy.Enhancer;

/**
 * @program: MvcProject
 * @description: 测试CGLIB动态代理：代理对象
 * @author: gujintao
 * @create: 2024-10-25 23:29
 **/
public class ProxyExample {
    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        // 设置代理类
        enhancer.setSuperclass(GreetingService.class);
        // 设置回调处理器
        enhancer.setCallback(new LoggingMethodInterceptor());
        // 创建代理对象
        GreetingService proxy = (GreetingService) enhancer.create();
        // 通过代理对象调用方法
        proxy.greet("world");
    }
}
```

- 什么时候采用哪种动态代理？

如果目标对象实现了接口，默认情况下会采用JDK的动态代理实现AOP。

如果目标对象实现了接口，可以强制使用CGLIB实现AOP。

如果目标对象没有实现了接口，必须采用CGLIB库。

- 两者的区别：

jdk动态代理使用jdk中的类Proxy来创建代理对象，它使用反射技术来实现，不需要导入其他依赖。

cglib需要引入相关依赖：asm.jar，它使用字节码增强技术来实现。

当目标类实现了接口的时候Spring Aop默认使用jdk动态代理方式来增强方法，没有实现接口的时候使用cglib动态代理方式增强方法。

# 什么是IOC？

IOC，Inversion Of Control，控制反转，由Spring容器管理Bean的整个生命周期。

通过反射实现对其他对象的控制，包括初始化、创建、销毁等，解放手动创建对象的过程，同时降低类之间的耦合度。

IOC的好处：降低了类之间的耦合，对象创建和初始化交给Spring容器管理，在需要的时候只需向容器进行申请。

# IOC的优点是什么？

1. IOC和依赖注入降低了应用的代码量。

2. 松耦合。

3. 支持加载服务时的饿汉式初始化和懒加载。

# 什么是DI？

DI，Dependency Injection，依赖注入，在Spring创建对象的过程中，把对象依赖的属性注入到对象中。

依赖注入实现了控制反转的思想。

Bean管理：Bean对象的创建以及Bean对象中属性的赋值（或者叫做Bean对象之间关系的维护）。

依赖注入主要有两种方式：构造器注入和属性注入。

1. 构造器注入：有参数构造器。

优点：强制依赖，保证了对象的完整性和不可变性。

缺点：对于大量依赖的情况，构造函数可能会变得很长且难以管理。

2. 属性注入：无参数构造器、setter方法。

优点：灵活性高，可以在运行时更改依赖。

缺点：依赖关系不是强制性的，可能导致部分依赖未被初始化的问题。

# Bean的生命周期？

1. 实例化：Spring容器根据配置文件或注解实例化Bean对象。

2. 属性注入：Spring将依赖(通过构造器、setter方法或字段注入)注入到Bean实例中。

3. 初始化前的扩展机制：如果Bean实现了BeanNameAware等aware接口，则执行aware注入。

4. 初始化前(BeanPostProcessor，后置处理器)：在Bean初始化之前，可以通过BeanPostProcessor接囗对Bean进行一些额外的处理。

5. 初始化：调用InitializingBean接口的afterPropertiesSet()方法或通过init-method属性指定的初始化方法。

6. 初始化后(BeanPostProcessor，后置处理器)：在Bean初始化之后，可以通过BeanPostProcessor进行进一步的处理。

7. 使用Bean：Bean已经初始化完成，可以被容器中的其他Bean使用。

8. 销毁：当容器关闭时，Spring调用DisposableBean接口的destroy方法或通过destroy-method属性指定的销毁方法。

# BeanFactory和FactoryBean的区别？

BeanFactory是个Factory，也就是IOC容器或对象⼯⼚，FactoryBean是个Bean。

在Spring中，所有的Bean都是由BeanFactory(也就是IOC容器)来进⾏管理的。 

但对FactoryBean⽽⾔，这个Bean不是简单的Bean，⽽是⼀个能⽣产或者修饰对象⽣成的⼯⼚Bean, 它的实现与设计模式中的⼯⼚模式和修饰器模式类似。

FactoryBean是Spring提供的一种整合第三方框架的常用机制。

和普通的Bean不同，配置一个FactoryBean类型的Bean，在获取Bean的时候，得到的并不是属性中配置的这个类的对象，而是实现FactoryBean后重写的getObject()方法的返回值。

# Spring自动装配的方式有哪些？

Spring的自动装配有三种模式：byType(根据类型)，byName(根据名称)、constructor(根据构造函数)。

若在IOC中，没有任何一个兼容类型的Bean能够为属性赋值，则该属性不装配，其值为默认值null。

如果找不到合适的Bean或者找到多个相同类型的Bean，则会抛出异常NoUniqueBeanDefinitionException。

1. byType

找到与依赖类型相同的Bean注入到另外的Bean中，这个过程需要借助setter注入来完成，因此必须存在set方法，否则注入失败。

2. byName

将属性名与Bean名称进行匹配，如果找到则注入依赖Bean。

3. constructor

Spring容器会查找与构造函数参数类型匹配的Bean，并将其注入。

# Spring中常见注解有哪些？

1. @Component

用途：通用的组件注解，用于标记一个类为Spring管理的Bean。

2. @Controller

用途：用于标记控制器类（处理HTTP请求）。

3. @Service

用途：用于标记服务层组件。

4. @Repository

用途：用于标记数据访问层组件。

5. @Qualifier

用途：当有多个相同类型的Bean时，使用@Qualifier来指定具体要注入的Bean。

6. @Value

用途：注入简单的值（如字符串、数字等），可以从属性文件中读取。

7. @Configuration

用途：标记一个类为配置类，通常包含@Bean方法。

8. @Bean

用途：在配置类中定义一个Bean，返回的对象将被注册到Spring容器中。

9. @Scope

用途：定义Bean的作用域，默认是单例（singleton）。常见的作用域有singleton, prototype, request, session等。

10. @PostConstruct

用途：标记初始化后的方法。

1·. @PreDestroy

用途：标记销毁前的方法。

12. @Transactional

用途：标记一个方法或类需要事务管理。

13. @RestController

用途：结合了@Controller和@ResponseBody，用于创建RESTful Web服务。

14. @RequestMapping

用途：通用映射，用于映射HTTP请求到控制器方法。

15. @GetMapping

用途：映射GET请求。

16. @PostMapping

用途：映射POST请求。

17. @PutMapping

用途：映射PUT请求。

18. @DeleteMapping

用途：映射DELETE请求

19. @Autowired

用途：自动注入依赖，默认根据类型。可以用在属性上、setter方法上、构造方法上、构造方法形参上、还能搭配@Qualifier注解一起使用。

属性注入，在service上加上@Autowired，根据类型找到对应的对象，完成注入。

```java
@Autowired
private UserService userService;
```

在setter方法上加上@Autowired。

```java
private UserService userService;

@Autowired
public void setUserService(UserService userService) {
    this.userService = userService;
}
```

在构造方法上加上@Autowired注入。

```java
private UserService userService;

@Autowired
public UserController(UserService userService) {
    this.userService = userService;
}
```

在构造方法的形参上注入@Autowired。

```java
private UserService userService;

public UserController(@Autowired UserService userService) {
    this.userService = userService;
}
```

只有1个有参数构造函数，注解@Autowired可省略。

```java
private UserService userService;

public UserController(UserService userService) {
    this.userService = userService;
}
```

一个接口有多个实现类，可以通过@Autowired和@Qualifier配合使用，根据名称进行注入。

```java
@Autowired
@Qualifier(value = "UserServiceImpl")
private UserService userService;

public UserController(UserService userService) {
    this.userService = userService;
}
```

20. @Resource

用途：自动注入依赖，根据名称。可以用在属性上、setter方法上。

指定名称，按照指定名称注入@Resource。

```java
@Resource(name = "userService")
private UserService userService;
```

不指定名称，按照属性名称注入@Resource。

```java
@Resource
private UserService userService;

@Service(value = "userService")
public Class UserServiceImpl implements UserService(){
    ...
}
```

# @Autowired和@Resource的区别？

1. 装配方式

@Autowired注解是默认按照类型（byType）装配依赖对象的。如果存在多个类型⼀致的Bean，⽆法通过byType注⼊时，就会使⽤byName来注⼊，如果还是⽆法判断注⼊哪个Bean，则会抛出异常UnsatisfiedDependencyException。 

@Resource会⾸先按照名称（byName）来装配；如果没有指定名称，则按照属性名称来找；如果找不到属性名称的Bean，才会使用byType来注入。

2. 所属

@Autowired注解是Spring框架自己的；@Resource注解是JDK扩展包中的。

3. 作用范围

@Autowired注解用在属性上、setter方法上、构造方法上、构造方法形参上、还能搭配@Qualifier注解一起使用。

@Resource注解用在属性上、setter方法上。

# @Bean和@Component有什么区别？

都是使用注解定义Bean。

@Bean是使用Java代码装配Bean，@Component是自动装配Bean。

1. @Component注解用在类上，表明一个类会作为组件类，并告知Spring要为这个类创建Bean，每个类对应一个Bean。

2. @Bean注解用在方法上，表示这个方法会返回一个Bean。@Bean需要在配置类中使用，即类上需要加上@Configuration注解。

# Lombok常用注解有哪些？

1. @Getter

用途：自动生成类中所有字段的getter方法。

2. @Setter

用途：自动生成类中所有字段的setter方法。

3. @NoArgsConstructor

用途：生成一个无参构造器。

4. @AllArgsConstructor

用途：生成一个包含所有字段的全参构造器。

5. @RequiredArgsConstructor

用途：生成一个包含所有final字段和带有@NonNull注解的字段的构造器。

6. @ToString

用途：生成toString方法，默认情况下会包含所有非静态字段。

7. @EqualsAndHashCode

用途：生成equals和hashCode方法，默认情况下会包含所有非静态字段。

8. @Data

用途：组合注解，包含了@Getter,@Setter,@ToString,@EqualsAndHashCode和@RequiredArgsConstructor。

9. @Builder

用途：为类生成一个内部构建器（Builder）模式。

10. @Slf4j

用途：为类生成一个SLF4J日志对象。

11. @Log

用途：为类生成一个java.util.logging日志对象。

12. @Log4j

用途：为类生成一个Log4j日志对象。

13. @SneakyThrows

用途：允许在方法中抛出受检异常（checked exception），而不需要在方法签名中声明。

14. @Value

用途：类似于@Data，但生成的是不可变类（所有字段都是final的）。

# Spring事务实现方式有哪些？

Spring事务机制主要包括声明式事务和编程式事务。

使用@Transactional注解开启声明式事务。

# 有哪些事务传播行为？

1. REQUIRED (默认)

如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

这是@Transactional注解的默认传播行为。

2. SUPPORTS

如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务方式执行。

适用于那些不需要事务支持的方法，但可以参与已存在的事务。

3. MANDATORY

如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常(IllegalTransactionStateException)。

适用于必须在事务中执行的方法。

4. REQUIRES_NEW

创建一个新的事务，如果当前存在事务，则将当前事务挂起。

新的事务独立于现有的事务，新的事务提交或回滚不会影响原有的事务。

5. NOT_SUPPORTED

以非事务方式执行操作，如果当前存在事务，则将当前事务挂起。

适用于那些不应该在事务中执行的方法。

6. NEVER

以非事务方式执行，如果当前存在事务，则抛出异常 (IllegalTransactionStateException)。

适用于那些绝对不能在事务中执行的方法。

7. NESTED

如果当前存在事务，则在嵌套事务内执行；如果当前没有事务，则创建一个新的事务。

嵌套事务可以独立于外部事务进行回滚，而不会影响外部事务的状态。这通常通过保存点（Savepoint）机制来实现。


使用required，内层事务与外层事务共用一个事务，出现异常同时回滚。

使用required new时，内层事务与外层事务是两个独立的事务。一旦内层事务进行了提交后，外层事务不能对其进行回滚。两个事务互不影响。

# Spring的单例Bean是否有线程安全问题？

1. 有实例变量的Bean是有状态的Bean，是非线程安全的。

2. 没有实例变量的对象是无状态的Bean，是线程安全的。

Bean在多线程环境下不安全，一般用Prototype模式或者使用ThreadLocal解决线程安全问题。

# 什么是循环依赖？

