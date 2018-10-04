---
title: Markdown Tutorial
date: 2018-01-23
categories: rookie
tags:
- Markdown
- tools
---

## 什么是 Markdown

Markdown 是一种轻量级标记语言，它由 [Aaron Swartz](https://zh.wikipedia.org/wiki/%E4%BA%9A%E4%BC%A6%C2%B7%E6%96%AF%E6%B2%83%E8%8C%A8) 和 [John Gruber](https://zh.wikipedia.org/wiki/%E7%B4%84%E7%BF%B0%C2%B7%E6%A0%BC%E9%AD%AF%E4%BC%AF) 共同设计。它允许人们「使用易读易写的纯文本格式编写文档，然后转换成有效的 XHTML（或者 HTML）文档」。

## 为什么要用 Markdown

> 1. 干净的纯文本，简单易学，可读性强，让使用者可以更好地专注于文字创作本身。
> 2. 跨平台；越来越多的网站支持 Markdown 语法，历史进程不可阻挡。
> 3. 兼容 HTML 语法。

## 基础语法

### 标题

n 级标题，以 n 个 **#** 号开头，支持一到六级标题。（# 号与标题内容之间需要加空格）
```
# 一级标题
## 二级标题
### 三级标题
###### 六级标题
```

实际效果：
> # 一级标题
> ## 二级标题
> ### 三级标题
> ###### 六级标题

### 水平分割线

**连续三个或以上的中划线 `-` 或星号 `*` 或下划线 `_`**（当前后都有段落时，前后最好都空出一行）。
```
---
```

实际效果：

---

### 换行

> 在行尾输入两个或以上的空格，然后回车。

### 引用块

**右尖括号「>」+ 空格**。根据排版渲染出来的实际效果，可以选择只在开头加一个或者每行前面都加一个。引用可以嵌套，根据嵌套层次加相应数量的符号即可。
```
> 外层引用的第一行  
外层引用的第二行
>> 内层嵌套引用的第一行  
内层嵌套引用的第二行
> 
> 外层引用的第三行。上面需要一个空行表示内层嵌套引用的结束，空行前面的「>」可有可无
```

实际效果：
> 外层引用的第一行  
外层引用的第二行
>> 内层嵌套引用的第一行  
内层嵌套引用的第二行
> 
> 外层引用的第三行。上面需要一个空行表示内层嵌套引用的结束，空行前面的「>」可有可无

### 强调

```
两侧各加上一个星号：*斜体*  
两侧各加上两个星号：**粗体**  
两侧各加上三个星号：***粗斜体***  
两侧各加上一个反引号：`高亮`  
两侧各加上两个波浪号：~~删除线~~
```

实际效果：
> 两侧各加上一个星号：*斜体*  
两侧各加上两个星号：**粗体**  
两侧各加上三个星号：***粗斜体***  
两侧各加上一个反引号：`高亮`  
两侧各加上两个波浪号：~~删除线~~

### 列表

#### 有序列表

`1. 有序列表 1（数字 + . + 空格）`  
`2. 有序列表 2`  
`8. 有序列表 3（数字不对也没关系，渲染时会自动变成正确的序号）`

实际效果：
> 1. 有序列表 1（数字 + . + 空格）  
> 2. 有序列表 2  
> 8. 有序列表 3（数字不对也没关系，渲染时会自动变成正确的序号）

#### 无序列表

```
- 无序列表 1（中划线 + 空格）
  - 嵌套无序列表 1（一个缩进）
      - 嵌套无序列表 2（两个缩进）
+ 无序列表 2（加号 + 空格）
* 无序列表 3（星号 + 空格）
```

实际效果：
> 
- 无序列表 1（中划线 + 空格）
  - 嵌套无序列表 1（一个缩进）
      - 嵌套无序列表 2（两个缩进）
+ 无序列表 2（加号 + 空格）
* 无序列表 3（星号 + 空格）

### 链接

```
行内式链接（推荐！易读，一眼就能看穿要跳到的链接地址）：[Inline-style link](https://github.com/)  
参考式链接 1：[Reference-style link 1][777]  
参考式链接 2：[Reference-style link 2][Reference-Text]  
// 参考式链接的实际链接地址一般都放在文档最后的脚注再赋值，前面放上链接引用标签区分
[777]: https://zh.wikipedia.org/wiki/Markdown
[Reference-Text]: https://en.wikipedia.org/wiki/Markdown
```

实际效果：
> 行内式链接（推荐！易读，一眼就能看穿要跳到的链接地址）：[Inline-style link](https://github.com/)  
参考式链接 1：[Reference-style link 1][777]  
参考式链接 2：[Reference-style link 2][Reference-Text]

### 图片

和链接相似，在链接的基础上，前方加一个感叹号 `！` 即可：`![img 标签的 alt 属性，一般填写图片名称](图片地址)`。
```
![图片名称](https://i.loli.net/2018/10/04/5bb4ee1624ea0.png)
```

实际效果：  
![图片名称](https://i.loli.net/2018/10/04/5bb4ee1624ea0.png)

## 进阶语法

### 表格

```
| name | age | hobby |
| :--- | ---: | :---: |
| :--- 表示左对齐 | ---: 表示右对齐 | :---: 表示居中 |
| Li lei  | 4 | sports |
| Han Meimei | 55 | music |
| Jack Ma | 666 | money |
```

实际效果：

| name | age | hobby |
| :--- | ---: | :---: |
| :--- 表示左对齐 | ---: 表示右对齐 | :---: 表示居中 |
| Li lei  | 4 | sports |
| Han Meimei | 55 | music |
| Jack Ma | 666 | money |

### 代码块高亮

开始和结束各使用三个反引号 \`\`\` 包裹一段代码，并在开始的三个反引号 \`\`\` 后指定一种语言（不同平台的代码高亮样式会有所不同）。
> \`\`\`Go  
> package main  
>   
> import "fmt"  
>   
> func main() {  
>  fmt.Println("Hello, World!")  
> }  
> \`\`\`  
>   
> \`\`\` javascript（部分平台可能需要先加一个空格再指定语言）  
> var s = "JavaScript";  
> console.log(s);  
> \`\`\`  

实际效果：
```Go
package main

import "fmt"

func main() {
  fmt.Println("Hello, World!")
}
```

``` javascript
var s = "JavaScript";
console.log(s);
```

### 复选框

可用来实现 Todo List 功能。
```
- [x] 已完成事项 1
- [x] 已完成事项 2
- [ ] 待办事项 1
- [ ] 待办事项 2
- [ ] 待办事项 3
```

实际效果：

- [x] 已完成事项 1
- [x] 已完成事项 2
- [ ] 待办事项 1
- [ ] 待办事项 2
- [ ] 待办事项 3

### 转义符

反斜杠 `\`。当在文档中需要使用到 Markdown 的符号（比如 `- # *`），不想它被转义，则在符号前加上反斜杠即可。
```
\*\*不想加粗\*\*
```

实际效果：
> \*\*不想加粗\*\*

## 特殊语法

以下特殊语法考虑到不同平台的支持限制，故只说明语法，不再展示效果（实际效果可以在支持该语法的平台中测试）。

### 脚注

```
脚注[^keyword]
// 脚注和参考式链接的赋值类似，一般都推荐放在文档的最后再赋值
[^keyword]: 这是一个示例脚注。
```

### LaTeX 公式

```
行内公式：$E=mc^2$  
块级公式：  
$$E=mc^2$$
```

### 流程图

> \`\`\`flow  
> st=>start: Start  
> op=>operation: Your Operation  
> cond=>condition: Yes or No?  
> e=>end  
>   
> st->op->cond  
> cond(yes)->e  
> cond(no)->op  
> \`\`\`

更详细的[流程图语法](http://flowchart.js.org/)。

### 时序图

> \`\`\`sequence  
> Alice->Bob: Hello Bob, how are you?  
> Note right of Bob: Bob thinks  
> Bob-->Alice: I am good thanks!  
> \`\`\`

更详细的[时序图语法](http://bramp.github.io/js-sequence-diagrams/)。

## Tips

- 各个平台对 Markdown 个别语法的支持可能会有其局限性（比如 LaTeX 公式），但基础语法都是通用的。
- 不同平台实际渲染出来的效果会有细节上的差异，请根据实际情况稍作调整。
- 调整样式的三大独门技巧：
    - 空格
    - 缩进
    - 换行

## 工具

- Web 端：[Cmd Markdown](https://www.zybuluo.com/) 和[马克飞象](https://maxiang.io/)
- 跨平台客户端：[Typora](https://typora.io/)
- Web 插件：[Markdown Here](https://github.com/adam-p/markdown-here)
- GitHub Issues 的输入框！

## Reference

- [Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax)
- [Markdown - Wikipedia](https://en.wikipedia.org/wiki/Markdown)
- [Markdown - 维基百科](https://zh.wikipedia.org/wiki/Markdown)
- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

[777]: https://zh.wikipedia.org/wiki/Markdown
[Reference-Text]: https://en.wikipedia.org/wiki/Markdown
