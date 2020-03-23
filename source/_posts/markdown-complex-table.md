title: markdown 插入复杂表格
tags:
  - markdown
  - table
mathjax: false
categories: 排版
date: 2019-09-20 23:20:11
---
*（批量导入网页上的表格）*
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/above-art-background-733852.jpg)
<!--MORE-->

## 目录
- 粘贴外来表格
- 处理表格内特殊字符（空格和"|"）
markdown 插入表格不支持合并单元格

当需要导入的表格太大时markdown手动输入工作量大不太友好，而利用 html 或者 excel 复制表格后粘贴至markdown 编辑器可以省去繁杂的编辑。具体操作见下面的示例：
## 粘贴外来表格
目标：将下图浏览器页面里的“混合字体”表格导入 markdown 编辑器 （本文作者用的是 typora 编辑器）

​		![](https://raw.githubusercontent.com/cogito0823/photos/master/img/prevtable.jpg)

首先用鼠标选中要复制的内容

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/copy.png)

右键点击复制

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/22223s.png)

打开 Excel 新建空白工作簿，在左上格子 `ctrl + v` 粘贴

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920223049.png)

表格大的话要耐心等待一些时间，即使提示无响应也不要关闭 Excel。粘贴好以后如下

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920223316.png)

表格右列是 mathjax 公式，需要将右列所有单元格首尾添加 `$`  ,操作如下

选中右列单元格，可以整列，也可以选择需要的几行，右键后选择设置单元格格式

​	![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920223703.png)

弹出设置单元格格式对话框，选择左侧“数字”-“自定义” 在类型里输入 `$@$` ，聪明的你猜出了其中的 `@` 表示单元格中的内容😉 ，然后点击确定回到工作簿页面发现选中的列各单元格首位已经加好了`$` 符号

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/SharedScsssreenshot.jpg)

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920224843.png)

接下来选中所有单元格右键复制后打开你的 markdown 编辑器（typora 编辑器不要在源代码模式进行）右键粘贴，你惊奇地发现表格都粘贴好啦：

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920225437.png)



typora编辑器的源代码模式如下图，左下角会提示退出源代码模式，如果你的是源代码模式那么 `ctrl + /`

可以退出。

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/156g8991451191.png)

我们可以看到粘贴好的表格里有部分单元格里出现了多余空行，部分单元格内没有居中对齐😕，改正这些地方需要 `ctrl + /` 进入“源代码模式”，粘贴的表格变成下图所示：

![](https://raw.githubusercontent.com/cogito0823/photos/master/img/20190920230144.png)

在那些长串的 `---------` 那修改对齐方式，如

- ---------: 设置内容和标题栏居右对齐。
- :--------- 设置内容和标题栏居左对齐。
- :--------: 设置内容和标题栏居中对齐。

然后将这段代码里的所有灰色和红色内容全部删除，注意红色部分里都有感叹号 `!` ，不要忽略。

删除以后再退出源代码模式，这时表格内容对齐了，大功告成~😁

#### 处理表格内特殊字符（空格和"|"）
markdown 将表格里的 `|` 符号视为表格格式符，因此表格里需要出现时 `|` 字符时需要用转义序列替代，`|` 的转义序列是`&#124;`,不要漏掉后面的英文分号。

当 `|` 符号出现在表格里的代码显示，即用在两个反引号` ` `中时，用`<code>` `</code>` 分别替代首位反引号即可。注意在两个 $ 号中时不需要替换成`<code>` `</code>`。