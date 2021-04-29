05.01 12:32
Termux wiki 1.0
Termux wiki 1.0


清水芷菱 2020-10-14


学习基础：对Linux操作系统与命令行有基本的了解，善用头脑和搜索引擎解决问题


前言

整篇教程建立于Termux官网的wiki文档
翻译的初衷是因为我初学termux时常在网上寻求帮助
一些困惑得到解决，但更多的问题因此诞生，归结于一点的话，就是——
他们是怎么知道能这么做的？
其实答案很简单，官网有提供说明文档

这篇教程是建立在英文版的说明文档上
因为能力有限，很多内容被我裁去不译了
比如如何搭建http服务，如何从GitHub获得termux未提供的模块，基于x11的界面等等
这些都是我还未学到的知识，所以——
这篇教程的初心就在于，看完这篇教程，你可以对termux具有初步的使用

尽管如此，我更推荐你看官网的Wiki文档
这不只因为我对WIKI内容进行了大胆的删减，节选
更出于私心，我所翻译的绝大多数内容是我所要钻研的
比如在Wiki文档中，有提供文本编辑器诸如nano emacs vim的讲解
但因为笔者是只学vim的，其他文本编辑器的讲解都被我忽视不译了

无论如何，如果你想学termux的话，希望这篇教程能在初期对你有所帮助
下面我们就开始


Termux Wiki

提供关于使用Termux应用程序及其软件包的技巧
这里提供的信息可能不适用于运行在Android5.x-6.x设备上的Termux安装
截至2020年01.01.2020，对这些Android版本的支持已被取消


介绍

Termux 是一个 Android 终端模拟器 和 Linux环境应用程序，直接工作而
不需要对手机进行额外的操作，比如获得root权限。它自动安装最小的基本系统，使用包管理器安装额外的包。

新手上路

它如何工作

Termux是一个终端模拟器应用程序，通过大量命令行实用程序移植到AndroidOS。其主要目标是将Linux命令行体验带给移动设备的用户，而不需要root或其他特殊设置。

终端模拟器本质上是一个应用程序，它调用系统将标准输入、输出和错误流重定向到显示器上

大多数Android操作系统上可用的终端应用程序，其实用程序非常有限
这些实用程序通常由操作系统或其他root工具(如Magisk)提供
我们决定更进一步，将在GNU/Linux系统上可用的通用软件移植到Android操作系统上

Termux既不是虚拟机，也不是任何其他类型的仿真或模拟环境
所有提供的软件包都是与AndroidNDK交叉编译的
只有兼容补丁才能让它们在Android上工作
因为操作系统不提供对其文件系统的完全访问,Termux无法将包文件安装到标准目录中
如 /bin、/etc、/usr或/var

 相反，所有文件都安装在位于
data/data/com.termux.com/files/usr

我们将该目录称为 $PREFIX ,它也是Termux shell中导出的环境变量
请注意，此目录不能更改或移动到SD卡，因为：

 文件系统必须支持Unix权限和特殊文件，如符号链接或套接字

 $PREFIX 路径被硬编码到所有二进制文件中



除了$PREFIX目录之外，用户还可以将文件存储在主目录($HOME)中



/data/data/com.termux/files/home

然而，termux与常规Linux发行版的差异并不只体现在文件系统上



Termux是Linux吗？


Termux提供了一个类似于Linux发行版的包生态系统
然而，您应该知道,termux只是一个运行在安卓操作系统上的常规应用程序。因此:

如前所述,所有内容都被安装到 $PREFIX 中,而不是像 /bin 或 /etc 这样的标准目录 环境仅限单用户使用。

作为root用户执行命令时要注意这一点

termux使用与安卓操作系统相同的libc和动态链接器


这三个主要的区别会在您试图运行典型的GNU / Linux系统编译程序时引起问题


我能用它做什么

Termux应用程序有许多常见的用法:

使用Python进行数据处理

在开发环境中编程

使用时间确定的工具下载和管理文件和页面

学习Linux命令行环境的基础知识

运行ssh客户机

同步和备份你的文件



当然,用法不仅限于上面所列——在我们的存储库中有超过1000个软件包
 如果这些软件包没有你要找的东西,你可以自己编译——我们提供多种构建工具：
 C、C + +、Go、Rust等语言的编译器
 NodeJS、Python、Ruby等常用语言的解释器
 
请注意,termux不是root工具,无法让您获得root权限

termux需要root权限吗？

通常termux不需要设备被root。事实上,它主要针对非root用户

您可能希望通过获得设备root来

修改设备的固件

操作操作系统或内核的参数

非交互式安装/卸载APK

对设备上的所有文件系统具有完全的R / W访问权限

可直接接入BT / Wi-Fi模块或串行线路等硬件设备

通过chroot在安卓上安装Linux发行版或集装箱化

可以“完全”控制您的设备


尽管如此，对于termux来说，获得root权限也不是必须的

还有其他教程吗

这一节是不完整的，如果你可以，请帮我们完善它
我们无法维护有关Linux命令、shell脚本和其他通用信息的全部文档
但可以为您提供指向外部资源的链接，如下



命令
发现更多Linux命令
https://linuxjourney.com/



shell 脚本
Shell脚本是使用终端的一个重要部分，这些是shell(Bash)脚本的指南
http://mywiki.wooledge.org/BashGuide
https://www.tldp.org/LDP/Bash-Beginners-Guide/html/



这些链接会对进阶用户有用。
https://wiki-dev.bash-hackers.org/

使用Bash内置命令的示例作为参考



我能为termux做些什么

作出贡献的最佳方式是：



改进TermuxWiki页面，即填充可以使用更多信息的部分，或纠正语法错误

提交错误报告。请只提交关于Termux软件包或应用程序的报告。其他错误应在其他地方提交

更新提交包，提交带有错误修复和改进的拉请求。


Termux的所有源代码可以通过下面网址找到
https://GitHub.com/termux



用户界面

termux提供的是一个终端界面,其文本大小可以通过手指对屏幕的缩放或拉动调整内容
除了终端,还有三个额外的界面元素可用:菜单、导航抽屉和通知

菜单可以通过长按终端上的任何地方来显示。菜单条目提供：

- 选择和粘贴文本
- 重置终端
- 退出当前终端会话
- 通过选择字体和配色对终端进行造型
- 显示这个帮助页面

屏幕左侧向右滑动以显示导航抽屉
(如果你在安卓中启用了手势导航,你需要在滑动之前短暂地按住屏幕边缘以打开导航抽屉,否则会返回)
它有三个元素：

- 点击一个会话让它在终端显示
- 长按允许您指定一个会话标题，长按命名会话
- 显示或隐藏扩展键
- 创建新终端会话的按钮

当终端会话在运行时,可以通过下拉通知菜单看到Termux通知
点击该通知将导致最当前的终端会话，通知也可以展开三个选项

- 退出所有正在运行的终端会话
- 使用唤醒锁避免进入睡眠模式
- 使用高性能WiFi锁来最大化WiFi性能

通过唤醒锁,即使没有终端会话运行,Termux的通知和后台进程也可以使用,
这使得服务器和其他后台进程运行得更稳定

图形界面

Termux具有基于X11的相当基本的图形用户界面支持


终端设置

termux终端可以通过在~/.termux目录下创建文件termux.properties 进行配置
此文件使用简单的 key=value 属性语法,并允许配置下面列出的属性

更改属性文件后,更改将通过执行**termux-reload-settings**
或重新启动termux应用程序(关闭所有会话并重新启动应用程序)生效
终端会话处理
可以设置快捷键组合来处理常见的会话操作。打开新会话、在会话之间切换和重命名会话可以调整如下：

- 打开一个新的终端ctrl + t (音量下键+ t)
shortcut.create-session = ctrl + t

- 切换至下个终端 (例如) ctrl + 2
shortcut.next-session = ctrl + 2

- 切换至上一终端 (例如) ctrl + 1
shortcut.previous-session = ctrl + 1

- 终端重命名 (例如) ctrl + n
shortcut.rename-session = ctrl + n


黑色主题

您可以通过配置
use-black-ui = true

如果系统用户界面使用暗主题，则在Android 9及更高版本上自动启用此功能


响铃字符处理

- 设备振动(默认).
bell-character=vibrate

- 响铃.
bell-character=beep

- 无反馈
bell-character=ignore

返回键处理

在按下返回键时可以设置返回键属性的配置如下：
- 按下esc键
back-key=escape

- 隐藏键盘或退出app(默认).
back-key=back

扩展键处理



Alt、Ctrl、ESC等按键的使用对于操作CLI终端是必要的。Termux触摸键盘并没有这些按键，为此，Termux使用音量下键按钮来模拟Ctrl键。例如，在手机键盘上同时按下音量下键+L与在硬件键盘上按Ctrl+L是发送相同的输入。
将Ctrl与键结合使用的效果是什么取决于所使用的程序，但对于绝大多数命令行工具，以下快捷方式是一样的效果：

- Ctrl+A
将光标移至行首

- Ctrl+C
阻止当前进程

- Ctrl+E
光标移至行尾

- Ctrl+U
从行首删至光标处

- Ctrl+K
从光标处删至行尾

- Ctrl+L
清屏

- Ctrl+Z
暂停当前进程

- Ctrl+W
向后删除一单词

- Ctrl+alt＋c
打开新的终端，只在黑客键盘上生效


音量上键

音量上键搭配其他按键时能提供特定的特殊按键

- Volume Up+E → Escape 键
- Volume Up+T → Tab 键
- Volume Up+1 → F1 (and Volume Up+2 → F2, etc)
- Volume Up+0 → F10
- Volume Up+B → Alt+B, 移至上一个单词
- Volume Up+F → Alt+F, 移至下一个单词
- Volume Up+X → Alt+X
- Volume Up+W → 上箭头
- Volume Up+A → 左箭头
- Volume Up+S → 下箭头
- Volume Up+D → 右箭头
- Volume Up+L → |
- Volume Up+H → ~
- Volume Up+U → _
- Volume Up+P → Page Up
- Volume Up+N → Page Down
- Volume Up+. → Ctrl+\ (SIGQUIT)
- Volume Up+V → 控制音量
- Volume Up+Q → 显示或隐藏扩展键
- Volume Up+K → 同上

扩展键

Termux还有一个扩展键排以扩展当前的键盘。要启用额外的键视图，您必须长按抽屉中的`KEYBROAD`按钮。
你也可以通过同时按音量上键＋Q键或音量上键＋K键
在Termuxv0.66版本之后，扩展键排可以通过文件“~/.termux/termux.properties”进行配置



软件



软件指的是在Android和Chrome系统的智能手机或平板电脑上termux所能工作的软件。Termux为您的设备提供广泛的软件。

集成开发环境

Termux是一个很棒的软件开发环境。
Termux经常用于软件开发、信息科学教育和实验。下面是一些文章和链接，介绍如何使用以下编程环境之一实现对软件的开发。

C
Package: clang
描述: C language frontend for LLVM
主页:

http://clang.llvm.org/

相关链接: 

https://github.com/termux/termux-packages/issues?q=clang

Python
Python是一种解释性的、高级的、通用的编程语言。Python由Guido van Rossum创建，于1991年首次发布。它的设计理念强调代码的可读性，并显著使用了重要的空格。它的语言构造和面向对象的方法旨在帮助程序员为小型和大型项目编写清晰、逻辑的代码。

在Termux中，安装Python可以通过
pkg install python
旧式版本2.7.x可以通过
pkg install python2

包管理

安装Python后，pip(pip2——如果使用python 2)包管理器也可以使用。下面介绍关于如何使用它的快速教程。
安装一个新的模块
pip install {module name}

卸载一个Python模块
pip uninstall {module name}

列出所有已安装的模块
pip list

在安装Python模块时，强烈建议安装包构建(这是必不可少的)，一些模块在安装过程中会编译设备的扩展

Python模块安装技巧
假设您已经做好安装包构建或者至少是安装好clang、make和pkg-config，
还假定Termux-Exec没有损坏，能在您的设备上正常工作。环境变量LD_preload未被篡改或取消设置。否则，您将需要修补模块的源代码，以修复所有的shebangs

LD_PRELOAD是Linux系统的环境变量，用于动态库的加载

- Numpy
pip命令安装

- pillow
pip命令安装


相关链接: 

https://github.com/termux/termux-packages/issues?q=python


与Linux的差异



Termux与常规的Linux发行版之间存在一些差异
Termux中的环境设置类似于现代Linux发行版的环境设置。然而，在Android上运行注定它会有几个重要的差异。

Termux不符合FHS标准
不像大多数Linux发行版，termux不遵循文件层次系统(fhs)。在常规位置你找不到/bin, /etc, /usr, /tmp等文件夹。
因此，所有程序应该进行修补和重新编译，以满足Termux环境，否则它们将无法找到它们的配置文件或其他数据。

您可能在执行具有标准Shebang(例如#!/bin/sh)的脚本时遇到问题。

Shebang通常出现在类Unix系统的脚本中第一行，作为前两个字符。在Shebang之后，可以有一个或数个空白字符，后接解释器的绝对路径，用于指明执行这个脚本文件的解释器。


在执行shebang命令之前，可以使用Terux-Fix-shebang脚本修改这些文件


Termux的最新版本提供了一个特殊的包(Terux-exec)，它让使用标准的shebang成为了可能


大多数包都具有共享库依赖项，这些依赖项被安装到$PREFIX/lib中

在Android 7之前的设备上，Termux导出特殊变量$LD_LIBRARY_PATH，该变量能告诉链接器在何处找到共享库的文件。



在Android7或更高的版本上，

使用DT_RUNPATHELF标头属性代替LD_LIBRAR_PATH。
如果由于某种原因您仍然需要经典的Linux文件系统布局，您可以尝试使用来自于 proot 包 的Terux-chroot：

pkg install proot
termux-chroot

如果您使用的自定义软件需要标准路径才能使用，则实用程序 Terux-chroot可能非常有用

根文件系统存储为普通应用程序数据
根文件系统和用户主目录位于位于/data分区上的私有应用程序数据目录中。这些目录的路径分别是$PREFIX和$HOME。
您不能将$PREFIX移动到其他位置，因为所有程序都希望$PREFIX不会更改。此外，您不能在SdCard上使用$PREFIX的二进制文件、符号链接和其他文件。原因很简单——termux文件系统不支持Unix权限、符号链接、套接字等.

重要：如果您卸载应用程序或擦除数据，目录$前缀和$HOME也将被删除。在进行此操作之前，请确保备份了所有重要数据。

Termux 是单用户的

Android应用程序被沙箱化，并拥有自己的Linux用户ID和SELinux标签。Termux也不例外，Termux中的所有内容都是使用与应用程序本身相同的用户id执行的

我们所有的包(只有root包除外)都被修补以删除任何多用户和其他类似的功能。我们还更改了服务器包的默认端口：ftpd、httpd和sshd的默认端口分别设置为8021、8080和8022

您可以读写所有应用程序组件，包括$PREFI包括$PREFIX。注意，这意味着你很容易因为不小心删除或覆盖$PREFIX中的文件破坏系统。

编辑器
对文本和数据文件编辑，编写和操作

文本编辑器
一个允许用户编辑文本的系统或程序。文本编辑器是用于编辑纯文本文件的一种程序类型。
文本编辑器提供操作系统和软件开发包，可用于更改配置文件、文档文件和编程语言源代码。

- Vim

在执行
apt install vim
之后的vim中键入以下命令
:help
来了解如何开始
对于有关VIM的常见问题，请输入
:help problem
如果您想学习如何使用Vim，可以在命令提示符下使用
vimtutor

Package: vim
主页: https://www.vim.org/
你可以安装vim，通过
pkg install vim

图形环境
如何在Termux中使用X 窗口系统的详情

意图和钩子
从其他应用程序使用意图和钩子来访问termux

包管理
基本和高级包和模块管理
Termux使用apt和dpkg进行包管理，类似于Ubuntu或Debian
在termux中更推荐使用pkg包管理工具，pkg包管理工具是对apt的封装。
它通过自动更新apt列表来简化软件包的安装或升级，这样您就不必在安装或升级包时键入

 apt update

安装包
pkg install [package name]
卸载包
pkg uninstall [package name]
列出所有包
pkg list-all
升级所有包
pkg upgrade

重要事项：在安装任何东西之前，确保所有软件包都是最新的。此外，我们建议每周至少检查一次更新。我们使用滚动发布更新风格，并且不关心当前版本和以前版本之间兼容性如何。如果您已经很长时间没有升级Termux安装包，又在运行包管理器时遇到错误，那么最简单的修复方法就是完成对所有包的升级并重新安装你想要安装的包。

有关可用命令的更多信息，您可以只运行不带参数的pkg，或者如下所示
pkg help


包事项

远程访问

访问远程设备或Termux本身
Termux能够使用一些常用工具访问远程设备。还可以将运行Termux的设备转换为远程控制服务器。

数据共享
用其他应用程序中共享家目录中的文件
默认情况下，其他应用程序将无法访问Termux中的家目录中的文件。这是Android本身的限制
有时，在您对一些文件进行修改时会有读取访问权限不足的情况。这可以通过将所需的文件存储在SDCARD上来实现

AndroidLollipop应该在框中使用，但是，MarshMoLlow及以上需要Termux应用程序请求读/写外部数据的权限

使用 termux-setup-storage 命令

该命令将让您显式允许termux获得读取文件的权限

在您允许后，该命令将在家目录下创建名为storage的文件夹
这个文件通过软链接指向设备的文件夹



shells
Termux提供能用的shell的列表
shell是命令语言解释器，其执行来自标准输入设备（如键盘）或文件的命令。shell不是系统内核的一部分，但可以使用系统内核执行程序，创建文件等
使用chsh命令能通过Termux-Tools更改登录的shell。目前Termux支持Bash，Fish，TCSH，ZSH和其他一些shell。

BASH
主页: https://www.gnu.org/software/bash/

Bash 是安装termux后默认的shell

BASH shell 初始化文件在  ~/.bashrc, $PREFIX/etc/bash.bashrc
 使用 `man bash` and `info bash` 查看更多信息


ZSH
主页: https://www.zsh.org/
安装命令: pkg install zsh
ZSH是一个专为交互式使用而设计的shell，但它也是一种强大的脚本语言。Bash，KSH和TCSH的许多有用功能被纳入ZSH。zsh shell init文件在~/.zshrc and $PREFIX/etc/zshrc and more. 更多信息通过'man zsh' 与 ' info zsh ' 查阅



硬件

硬件键盘

将Termux与硬件（例如蓝牙）键盘一起使用时，可以使用以下快捷方式，方法是将它们与Ctrl+Alt组合使用：
'C'→创建新会话
'R'→重命名当前会话
向下箭头（或“N”）→下一个会话
向上箭头（或“P”）→上一个会话
右箭头→打开抽屉
左箭头→关闭抽屉
‘M’→显示菜单
“U”→选择URL
“V”→粘贴
+/-→调整文本大小
1-9→转到编号的会话

硬件鼠标

你几乎可以用触摸屏上的硬件鼠标做任何你能做的事情，即虚拟鼠标

内部和外部存储

Termux中有三种主要的存储类型：

- App private storage：存放在$HOME中的文件，可从Termux的文件夹获得

- 共享内部存储：设备中供所有应用程序使用的存储空间。在Android 6.0以上的版本，这需要用户授予Termux访问权限

- 外部存储：存储在外部SD卡上。每个应用程序在外部SD卡上都有一个专用文件夹，它们之间的交换需要使用Termux中尚未提供的特殊API。基于SAF的访问是不可能的，因为shell是在Android框架之外执行的



访问共享内部存储和外部存储

要访问共享内部存储，您需要执行**termux setup storage**命令
然后系统会提示您“允许Termux访问设备上的照片、媒体和文件”，您应该允许这样做

允许termux读取存储权限可确保：

- 在Android 6.0或更高版本上运行时，你能用termux读取或写入内部存储

- 创建外部存储上的应用程序专用文件夹（如果存在外部存储）。

- 创建文件夹$HOME/storage，该文件夹为指向了共享内部存储的软链接

内外存储的各文件夹描述

- ~/storage
创建的$HOME/storage文件夹的内容是指向不同存储文件夹的符号链接

- ~/storage/shared
所有应用程序之间共享存储的根。

- ~/storage/downloads
从系统浏览器下载的标准目录。

- ~/storage/dcim
照相机图片和视频所存在的标准目录。

- ~/storage/pictures
用于放置用户可用图片的标准目录。

- ~/storage/music
标准目录，用于放置用户的常规音乐列表中的任何音频文件。

- ~/storage/movies
用于放置用户可用影视资源的标准目录。

- ~/storage/external-1
指向外部存储器上的Termux专用文件夹的符号链接（仅当外部存储器可用时）。

- 从文件管理器访问Termux
您可以使用文件管理器访问Termux主目录（$home），并且能够以读写模式访问USB或外部SD卡之类的驱动器

怎么获得安装包

- 谷歌商店

- F-droid

- 酷安(不推荐)


~ $ termux-setup-storage

Directory '~/storage' is going to be wiped. No storage contents will be touched.

Do you want to continue ? (y/n) y
~ $ cd
~ $ ls
storage
~ $ ls  /data/data/com.termux/files/home
storage
~ $ ls  /data/data/com.termux/files/home/
storage
~ $ ls  /data/data/com.termux/files/home/storagedcim       movies  pictures
downloads  music   shared
~ $ ls $HOME
storage
~ $ ls $HOME/
storage
~ $ ls $HOME/storage/
dcim       movies  pictures
downloads  music   shared
~ $ ls $HOME/storage
dcim       movies  pictures
downloads  music   shared
~ $ ls  $PREFI
storage

~ $ ls  $PREFI
storage
~ $ ls $PREFI/storage
emulated  otg  self
~ $ ls $PREFI/storage/otg
sda  sdb  sdc  sdd  sde  sdf












