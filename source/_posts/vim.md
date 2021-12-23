---
title: 什么是VIM #文章页面上的显示名称，可以任意修改，不会出现在URL中
date: 2021-10-15 14:22:16 #文章生成时间，一般不改，当然也可以任意修改
categories: #文章分类目录，可以为空，注意:后面有个空格
tags: [Vim,bash,精品] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
description: VIM介绍及常用命令
---

# 什么是vim
一个**1991年**正式发布，如今已经快30岁的「高龄」的代码编辑器Vim
Vi源自QED编辑器，距今已有五十多年的历史。简单说，其发展历程如下：

-   1966年：伯克利分时系统的QED（“Quick EDitor”）
-   1969年8月：QED ->　AT&T的ed    -- 与vim的命令类似
-   1976年2月：ed ->玛丽王后大学的em（“Editor for Mortals”）
-   1976年：em -> 加州大学伯克利分校的ex (“EXtended”)
-   1977年10月：ex有了可视化模式，vi

https://www.bilibili.com/read/cv9065974
## Vim的前身——Vi

基于em代码，Sun联合创始人，兼首席科学家Bill Joy开发了ex，算得上是扩展版。它在以往的模式上增加了**visual**模式，它可以在屏幕上打开文件。

三年之后，操作系统中引入了可执行文件Vi，但仍然可以通过在Vi/Vim访问ex命令
当时Bill Joy使用的是下面的键盘：esc键在现在的tab位置，方向键和字母键混用，这也就注定了之后Vim怪异的键位设计。
![[vim_keyboard.png]]

## Vim的诞生

这还得从Vi发布之后的「模仿」开始，很多人开始模仿、克隆vi编辑器。当时就有这么一个「Vi Improved」从中脱颖而出。

它是由「Bram Moolenaar」创建，vim改善了vi的兼容性，提供了更多的命令。
# 为什么要使用vim
![[vim_rank.png]]
参考：https://insights.stackoverflow.com/survey/2021#worked-with-vs-want-to-work-with-new-collab-tools-worked-want
## Vim 到底好在哪里
- Vim 是一个完全跨平台的编辑器。多平台快捷键高度一致，无需重复学习
- Vim 也是一个高度可定制、可扩展的编辑器。
- Vim 在变化的世界中相对静止

个人理解：
- 丰富的快捷键，使编辑更高效
- 插件支持
- 更优雅的代码输入方式
   - 专注
  - 极简
  - 高效
# 如何使用vim
Vim诞生的时候，鼠标还不是电脑标配，所以Vim尽量为纯键盘操作而优化。在stackoverflow上有一个长盛不衰的问题：如何退出vim？
![[vim_how_do_i_exit_the_vim.png]]
vim以非常陡峭的学习曲线著称，对于新手，我推荐vim插件+主流编辑器方式学习
![[vim_studyhard.png]]
目前这种方式也正在流行。
![[vim_connect.png]]
# esc 命令模式切换

在大多数编辑器中，相信大家都喜欢敲几个单词就“保存（ctrl+s）”一下。而在vim中，保存是`:w`，而且需要在命令模式下进行。因此，往往要按`Esc:w`多达三个键才能保存。很多初学者十分诟病这个设计。事实上，经常使用`Esc`切换到命令模式才是vimer需要练就的第一个重要的反射行为。可以毫不夸张的说，只要你不在输入文字，就应该切换在命令模式下，命令模式应该是常态！

# hjkl

`h` 向前移动一个字符

`j` 向下移动一行

`k` 向上移动一行

`l` 向后移动一个字符

# 光标移动

`w`、`e`、`b`：按照单词进行前后光标跳转，也可以组合数字进行跳转，不过以我的经验，与其去算要跳多少个单词，不如多按几次吧。

`I`、`A`：移动到行首或行末的第一个字符处，并进入插入模式。

`Ctrl+D`、`Ctrl+U`：有时，需要看的文本不在可视区域，通过这些组合进行上下翻页。

`^`、`$`、`0`：光标移动到行首和行尾（0是绝对行首）。不过因为`^`和`$`都需要同时按住shift，而且数字键我们往往难以盲打，** 所以我一般直接使用`I+Esc`、`A+Esc`。**

`%`：移动到与当前括号匹配的括号处。

`f`、`F`：通过上面的例子，我们知道，`f`是find的意思，可以在一行内查找某个字符出现的位置，并直接跳转过去。比如`f<`可以从当前光标开始向右，找到第一个`<`，并移动过去。F是向左查找。

`G` : 跳转到当前文档最后一行

**重点**：经常会点错导致光标飞了，此时按`ctrl+o`即可返回上一位置

`shift + >` visual选中的内容整体右移，默认一个tab距离。同理`shift +　＜`整体左移
`shift +　num + >` 光标及以下num行向右移动

## 屏幕滚动
`ctrl + f` 向下翻页

`ctrl + b` 向上翻页

`ctrl + e` 逐行下滚

`ctrl + y` 逐行上滚

`H`、`M`、`L`：光标分别跳转到可视区域的最上面、中间、最下面。

`zt` 置顶当前行

`zz` 将当前行移动到屏幕中间

`zb` 移动到底部

# 插入

* `i` 、`a` : 都为从命令模式进入编辑模式，区别为`i`退出命令模式后会向前前进一个字符，`a`不会
* `I`、`A`：移动到行首或行末的第一个字符处，并进入编辑模式。
* `O`、`o` `O`在当前行上方新建一行，并进入编辑模式，`o`在当前行下方新建一行，进入编辑模式。

# 删除

* `dd`: 删除当前光标所在的一行
* `dw`: 删除一个单词
* `x` :  删除当前光标覆盖的字符
* `X`、`D`: 删除光标前的字符

# 撤销
`u`: 撤销上一次输入
`Ctrl+r` 回退前一个命令（即还原上次撤销）

# 复制

* `nyy`: 复制n行
* `yy`、`Y`: 复制当前行

# 高效修改

- `r`：替换模式，替换当前光标所在位置的一个字符。虽然你同样可以`i`进入插入模式，然后删掉那个字符，再输入需要的字符，但这种操作是鼠标流思维方式。替换是一个可重复操作，多用没坏处。
- `cw`：`change word`可以删除从当前位置到一个单词的结尾，并进入插入模式。这种操作常用于修改一个变量。比如对于：`int count=0;`希望把`count`改成`cnt`，那么当光标位于`c`字符处的时候，按`cw`可直接删除`count`，并进入插入模式。然后直接继续输入`cnt`即可。
- `caw`：`change a word`可以删除当前光标所在位置的单词。对于`int count=0;`的例子，如果此时光标在`count`中间某处，比如`u`处，直接键入`caw`可以达到同样的效果。所以`caw`更强大一些。
- 删除单词同样可以使用`diw`,与`caw`不一样的是`diw`删除后还是命令模式,`caw`进入编辑模式

#  查找
在normal模式下按下`/`即可进入查找模式，输入要查找的字符串并按下回车。 Vim会跳转到第一个匹配。按下`n`查找下一个，按下`N`查找上一个。

Vim查找支持正则表达式，例如`/vim$`匹配行尾的`vim`。 需要查找特殊字符需要转义，例如`/vim\$`匹配`"vim$"`。

> 注意查找回车应当用`\n`，而替换为回车应当用`\r`（相当于`<CR>`）。

## 大小写敏感查找

在查找模式中加入`\c`表示大小写不敏感查找，`\C`表示大小写敏感查找。默认大小写不敏感。例如：

```
/foo\c
```

将会查找所有的`"foo"`,`"FOO"`,`"Foo"`等字符串。

### 大小写敏感配置

Vim 默认采用大小写敏感的查找，为了方便我们常常将其配置为大小写不敏感：

```
" 设置默认进行大小写不敏感查找
set ignorecase
" 如果有一个大写字母，则切换到大小写敏感查找
set smartcase 
```

> 将上述设置粘贴到你的`~/.vimrc`，重新打开Vim即可生效。

# 替换
`:s`（substitute）命令用来查找和替换字符串。语法如下：

```
:{作用范围}s/{目标}/{替换}/{替换标志}
```

例如`:%s/foo/bar/g`会在全局范围(`%`)查找`foo`并替换为`bar`，所有出现都会被替换（`g`）

注意对于一些特殊字符比如小数点,斜杠,双引号等需要转义,方式是使用反斜杠,在需要转义的字符面前加一个反斜杠 如把`"123//"`替换`'123\\'`
命令如下:
`:s/\"123\/\/\"/\'123\\\\\'/g`

## 作用范围

作用范围分为当前行、全文、选区等等。

当前行：

```
:s/foo/bar/g
```

全文：

```
:%s/foo/bar/g
```

选区，在Visual模式下选择区域后输入`:`，Vim即可自动补全为 `:'<,'>`。

```
:'<,'>s/foo/bar/g
```

2-11行：

```
:5,12s/foo/bar/g
```

当前行`.`与接下来两行`+2`：

```
:.,+2s/foo/bar/g
```

## 替换标志

上文中命令结尾的`g`即是替换标志之一，表示全局`global`替换（即替换目标的所有出现）。 还有很多其他有用的替换标志：

空替换标志表示只替换从光标位置开始，目标的第一次出现：

```
:%s/foo/bar
```

`i`表示大小写不敏感查找，`I`表示大小写敏感：

```
:%s/foo/bar/i
# 等效于模式中的\c（不敏感）或\C（敏感）
:%s/foo\c/bar
```

`c`表示需要确认，例如全局查找`"foo"`替换为`"bar"`并且需要确认：

```
:%s/foo/bar/gc
```

回车后Vim会将光标移动到每一次`"foo"`出现的位置，并提示

```
replace with bar (y/n/a/q/l/^E/^Y)?
```

按下`y`表示替换，`n`表示不替换，`a`表示替换所有，`q`表示退出查找模式， `l`表示替换当前位置并退出。`^E`与`^Y`是光标移动快捷键，参考： [Vim中如何快速进行光标移动](https://harttle.land/2015/11/07/vim-cursor.html)。

# 全选编辑
`ggvG` `ggVG`:全选, `gg`：是让光标移到首行，在vim才有效，vi中无效, `v`是进入Visual(可视）模式, `G`光标移到最后一行
 `ggyG` :全部复制
 `ggdG` : 全部删除

 # :命令
 命令模式：
 `:wq`退出保存
 `:q!`强制退出不保存

`:e` 重新载入

# 宏
- `q`：进入recording模式：
1.  Start recording by pressing q, followed by a lower case character to name the macro
2.  Perform any typical editing, actions inside Vim editor, which will be recorded
3.  Stop recording by pressing q
4.  Play the recorded macro by pressing @ followed by the macro name
5.  To repeat macros multiple times, press : NN @ macro name. NN is a number
参考：https://www.thegeekstuff.com/2009/01/vi-and-vim-macro-tutorial-how-to-record-and-play/


参考：
https://pragmaticpineapple.com/how-did-vim-become-so-popular/
https://www.geekpanshi.com/archives/a7a1df11.html
https://insights.stackoverflow.com/survey/2021#worked-with-vs-want-to-work-with-new-collab-tools-worked-want
vim学习工具：https://openvim.com/