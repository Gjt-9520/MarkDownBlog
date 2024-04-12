---
title: "MarkDown基本语法"
date: 2023-11-26
description: ""
cover: https://github.com/Gjt-9520/ImageResources/blob/main/photos/original/Ximage93.jpg?raw=true
tags: ["MarkDown"]
category: "学习笔记"
updated: 2023-11-26
---

## [官方教程](https://markdown.com.cn/)

## 标题(Heading)

```
# H1
## H2
### H3
```

## 粗体(Bold)

```
**bold text**
```

## 斜体(Italic)

```
*italicized text*
```

## 引用块(Blockquote)

```
> blockquote
```

## 有序列表(Ordered List)

```
1. First item
2. Second item
3. Third item
```

## 无序列表(Unordered List)

```
- First item
- Second item
- Third item
```

## 代码(Code)

```
`code`
```

## 分隔线(Horizontal Rule)	

```
---
```

## 链接(Link)

```
[title](https://www.example.com)
```

## 图片(Image)

```
![alt text](image path)
```

或者

```
![alt text][alt text]

[alt text]:base64 image path
```

## 视频(Video)

```
<iframe width="100%" height="468" src="video path" title="YouTube video player" 
frameborder="0" allow="accelerometer;  autoplay;  clipboard-write;  encrypted-media;  
gyroscope;  picture-in-picture;  web-share" allowfullscreen></iframe>
```

## 表格(Table)

```
| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |
```

## 代码块(Fenced Code Block)

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

## 脚注(Footnote)

```
Here's a sentence with a footnote. [^1]
[^1]: This is the footnote.
```

## 标题编号(Heading ID)

```
### My Great Heading {#custom-id}
```

## 定义列表(Definition List)

```
### My Great Heading {#custom-id}
```
## 删除线(Strikethrough)

```
~~The world is flat.~~
```
## 任务列表(Task List)

```
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```