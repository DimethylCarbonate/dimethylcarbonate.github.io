---
layout:     post
title:      Markdown 扩展语法
subtitle:   MarkDown使用指北（2）
date:       2022-09-29
original:  True
author:     Tresol
header-img: "img/img8.jpg"
header-img-credit: Microsoft
header-img-credit-href: http://www.microsoft.com/
catalog: true
tags:
    - 笔记
    - 计算机语言
    - Markdown
---

# Markdown 扩展语法

[基本语法](https://tresol.github.io/2022/09/25/MDInstructions/)主要是为了应付大多数情况下的日常所需元素，但对于某些人来说还不够，这就是扩展语法的用武之地。

### Markdown 表格

要添加表，请使用三个或多个`-`创建每列的标题，并使用`|`分隔每列。您可以选择在表的任一端添加管道。

```
| 代码托管平台 | 名称        |
| ----------- | ----------- |
| 国内        | Gitee       |
| 国外        | Github      |
```

呈现的输出如下所示：

| 代码托管平台 | 名称        |
| ----------- | ----------- |
| 国内        | Gitee       |
| 国外        | Github      |

单元格宽度可以变化，如下所示。呈现的输出将看起来相同。

```
| 代码托管平台    | 名称        |
| ----------- | ----------- |
| 国内    | Gitee    |
| 国外 | Github |
```

| 代码托管平台 | 名称        |
| ----------- | ----------- |
| 国内        | Gitee       |
| 国外        | Github      |

`提示` 使用连字符和管道创建表可能很麻烦。为了加快该过程，请尝试使用[Markdown Tables Generator](https://www.tablesgenerator.com/markdown_tables)。使用图形界面构建表，然后将生成的Markdown格式的文本复制到文件中。

#### 对齐

您可以通过在标题行中的连字符的左侧、右侧或两侧添加`:`，将列中的文本对齐到左侧，右侧或中心。

```
| 代码托管平台      | 名称 | 测试    |
| :---        |    :----:   |          ---: |
| 国内      | Gitee        | 123   |
| 国外   | Github        | 321     |
```
呈现的输出如下所示：

| 代码托管平台      | 名称 | 测试    |
| :---        |    :----:   |          ---: |
| 国内      | Gitee        | 123   |
| 国外   | Github        | 321     |

#### 格式化表格中的文字

可以在表格中设置文本格式。例如，您可以添加链接，代码（仅反引号`` ` ``中的单词或短语而不是代码块）和强调。不能添加标题，块引用，列表，水平规则，图像或HTML标签。

#### 在表中转义管道字符
您可以使用表格的HTML字符代码（`&#124;`）在表中显示竖线（|）字符。

### Markdown 围栏代码块
Markdown基本语法允许您通过将行缩进四个空格或一个制表符来创建代码块。如果发现不方便，请尝试使用受保护的代码块。根据Markdown处理器或编辑器的不同，您将在代码块之前和之后的行上使用三个反引号（` ``` `）或三个波浪号（`~~~`）。

``````
```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
``````
呈现的输出如下所示：

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

`提示`要在代码块中显示反引号？只要外面的反引号比里面的多就行！

#### 语法高亮
许多Markdown处理器都支持受围栏代码块的语法突出显示。使用此功能，您可以为编写代码的任何语言添加颜色突出显示。要添加语法突出显示，请在受防护的代码块之前的反引号旁边指定一种语言。

``````
```C++
include<bits/stdc++.h>
using namespace std;
int main(){
    cout<<"Welcome to Tresol Blog!"
    return 0;
}
```
``````

呈现的输出如下所示：

```C++
include<bits/stdc++.h>
using namespace std;
int main(){
    cout<<"Welcome to Tresol Blog!"
    return 0;
}
```

### Markdown 标题编号

许多Markdown处理器支持标题的自定义ID - 一些Markdown处理器会自动添加它们。添加自定义ID允许您直接链接到标题并使用CSS对其进行修改。要添加自定义标题ID，请在与标题相同的行上用大括号括起该自定义ID。

```
### My Great Heading {#custom-id}
```

HTML看起来像这样：

```
<h3 id="custom-id">My Great Heading</h3>
```

#### 链接到标题ID
通过创建带有数字符号（`#`）和自定义标题ID的`(标准链接)`，可以链接到文件中具有自定义ID的标题。

```
Markdown:[Heading IDs](#heading-ids)
HTML:<a href="#heading-ids">Heading IDs</a>	Heading IDs
```
其他网站可以通过将自定义标题ID添加到网页的完整URL（例如`[Heading IDs](https://markdown.com.cn/extended-syntax/heading-ids.html#headid)`）来链接到标题。

### Markdown 定义列表

一些Markdown处理器允许您创建术语及其对应定义的定义列表。要创建定义列表，请在第一行上键入术语。在下一行，键入一个冒号，后跟一个空格和定义。

```
第一个名词
: 这是第一个名词的定义。

第二个名词
: 这是第二个名词第一个定义。
: 这是第二个名词第二个定义。
```

HTML看起来像这样：

```
<dl>
  <dt>第一个名词</dt>
  <dd>这是第一个名词的定义。</dd>
  <dt>第二个名词</dt>
  <dd>这是第二个名词第一个定义。</dd>
  <dd>这是第二个名词第二个定义。</dd>
</dl>
```

呈现的输出如下所示：

第一个名词
: 这是第一个名词的定义。

第二个名词
: 这是第二个名词第一个定义。
: 这是第二个名词第二个定义。

### Markdown 删除线

您可以通过在单词中心放置一条水平线来删除单词。此功能使您可以指示某些单词是一个错误，要从文档中删除。若要删除单词，请在单词前后使用两个波浪号`~~`。

```
~~世界是平坦的。~~ 我们现在知道世界是圆的。
```

呈现的输出如下所示：

~~世界是平坦的。~~ 我们现在知道世界是圆的。

### Markdown 任务列表语法

任务列表使您可以创建带有复选框的项目列表。在支持任务列表的Markdown应用程序中，复选框将显示在内容旁边。要创建任务列表，请在任务列表项之前添加`-`和方括号`[ ]`，并在`[ ]`前面加上空格。要选择一个复选框，请在方括号`[x]`之间添加` x `。

```
- [x] 写好文章
- [ ] 升级网站
- [ ] 联系媒体
```

呈现的输出如下所示：

- [x] 写好文章
- [ ] 升级网站
- [ ] 联系媒体

### Markdown 使用 Emoji 表情

有两种方法可以将表情符号添加到Markdown文件中：将表情符号复制并粘贴到Markdown格式的文本中，或者键入emoji shortcodes。

#### 复制和粘贴表情符号

在大多数情况下，您可以简单地从[Emojipedia](https://emojipedia.org/) 等来源复制表情符号并将其粘贴到文档中。许多Markdown应用程序会自动以Markdown格式的文本显示表情符号。从Markdown应用程序导出的HTML和PDF文件应显示表情符号。

`提示`: 如果您使用的是静态网站生成器，请确保将HTML页面编码为UTF-8。

#### 使用表情符号简码

一些Markdown应用程序允许您通过键入表情符号短代码来插入表情符号。这些以冒号开头和结尾，并包含表情符号的名称。

```
去露营了！ :tent: 很快回来。

真好笑！ :joy:
```

呈现的输出如下所示：

去露营了！ :tent: 很快回来。

真好笑！ :joy:

`注意`：您可以使用此[表情符号简码列表](https://gist.github.com/rxaviers/7360908)，但请记住，表情符号简码因应用程序而异。有关更多信息，请参阅Markdown应用程序的文档。

### Markdown自动网址链接

许多Markdown处理器会自动将URL转换为链接。这意味着如果您输入`http://www.example.com`，即使您未使用方括号，您的Markdown处理器也会自动将其转换为链接。如果您不希望自动链接URL，则可以通过将URL表示为带反引号的代码来删除该链接。
