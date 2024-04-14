---
title: "MarkDown基本语法"
date: 2023-11-26
description: ""
cover: https://github.com/Gjt-9520/Resource/blob/main/Aimage-135/Aimage93.jpg?raw=true
tags: ["MarkDown"]
category: "学习笔记"
updated: 2023-11-27
swiper_index: 3
top_group_index: 3
---

## [官方教程](https://markdown.com.cn/)

## 标题(Heading)

```MarkDown
# H1
## H2
### H3
```

## 粗体(Bold)

```MarkDown
**bold text**
```

## 斜体(Italic)

```MarkDown
*italicized text*
```

## 引用块(Blockquote)

```MarkDown
> blockquote
```

## 有序列表(Ordered List)

```MarkDown
1. First item
2. Second item
3. Third item
```

## 无序列表(Unordered List)

```MarkDown
- First item
- Second item
- Third item
```

## 代码(Code)

```MarkDown
`code`
```

## 分隔线(Horizontal Rule)	

```MarkDown
---
```

## 链接(Link)

```MarkDown
[title](https://www.example.com)
```

## 图片(Image)

```MarkDown
![alt text](image path)
```

或者

```MarkDown
![alt text][alt text]

[alt text]:base64 image path
```

## 视频(Video)

```MarkDown
<iframe width="100%" height="468" src="video path" title="YouTube video player" 
frameborder="0" allow="accelerometer;  autoplay;  clipboard-write;  encrypted-media;  
gyroscope;  picture-in-picture;  web-share" allowfullscreen></iframe>
```

## 表格(Table)

```MarkDown
| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |
```

## 代码块(Fenced Code Block)

```MarkDown
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

## 脚注(Footnote)

```MarkDown
Here's a sentence with a footnote. [^1]
[^1]: This is the footnote.
```

## 标题编号(Heading ID)

```MarkDown
### My Great Heading {#custom-id}
```

## 定义列表(Definition List)

```MarkDown
### My Great Heading {#custom-id}
```
## 删除线(Strikethrough)

```MarkDown
~~The world is flat.~~
```
## 任务列表(Task List)

```MarkDown
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```