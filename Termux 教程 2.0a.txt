05.01 12:34
Termux 教程 2.0
Termux 教程 2.0


清水芷菱 1月8日

前言

呜呼哀哉！笔者因手贱使得手机数据重置。

Termux的安装连同所有配置化为乌有

现笔者重新配置Termux，在原Termux翻译文档的基础上作一次新的排版与增删

希望对学Termux的萌新有所帮助

注意:

因为一些地方是我自己的理解，并对原文有删改,所以在内容上无法做到完全正确，甚至会有与真相背道而驰的地方, 所以读者应该学会如何慎审地看待与学习

Termux 介绍
Termux 是一个模拟 Linux 环境，直接工作在 Android 的终端应用程序。它将 Linux 上的大量命令行与实用程序移植到了Android OS, 调用系统将标准输入、输出和错误流重定向到显示器上。

其主要目的是在不需要 root 或其他特殊设置的操作下,将 Linux 命令行体验带给移动设备的用户。

大多数 Android 系统上的终端应用程序，其使用的实用程序是非常有限的。Termux则更进一步，它把在GNU/Linux系统上很多的通用软件都移植到Android操作系统上

Termux的所有源代码可以通过下面网址找到 https://GitHub.com/termux

注意:

截至2020/01/01    Termux 对Android 7版本以下的设备不再支持

可以在谷歌商店（需翻墙＋账号），F-droid(推荐）,  APKPure（需翻墙，推荐），酷安（貌似版本落后）上下载该软件


它如何工作
Termux 不是虚拟机。它提供的所有软件包都是与Android NDK 交叉编译的，通过兼容补丁让它们在 Android 上工作。因为安卓操作系统不提供对其文件系统的完全访问，所以 Termux 无法将包文件如 /bin 、/etc 、/usr 、/var 放在标准目录中。

因此,这类包文件都安装在

data/data/com.termux/files/usr

Termux将该目录称为 $PREFIX , 它是 Termux shell 提供的环境变量

注意：不能更改或移动这个目录

除了提供 $PREFIX

用户还可以将文件存储在主目录(用户目录)

/data/data/com.termux/files/home

该目录被称作 $HOME ,也是 Termux shell 提供的环境变量

Termux与常规Linux发行版的差异并不只体现在文件系统上



Termux需要root权限吗？
通常 Termux 不需要设备被 root 。事实上,它主要针对非 root 用户

尽管获得根权限后能做很多事情，但对于 Termux 来说，获得 root 权限不是必须的

我能用它做什么
Termux 应用程序有许多常见的用法:

使用Python进行数据处理

在开发环境中编程

使用时间确定的工具下载和管理文件和页面

学习Linux命令行环境的基础知识

运行ssh客户机

同步和备份你的文件

当然，用法不限于上面所列

Termux的存储库中有超过1000个软件包。如果这些软件包没有你要找的东西,你可以自己编译——Termux提供多种构建工具：C、C + +、Go、Rust等常用语言的编译器Nodejs、Python、Ruby等常用语言的解释器

请注意，termux不是root工具。无法让您获得root权限

Termux是Linux吗？
Termux提供了一个类似于Linux发行版的包生态系统

然而, Termux只是一个运行在安卓操作系统上的常规应用程序。因此:

如前所述,包文件都被安装到 $PREFIX 中,而不是像 /bin 或 /etc 这样的标准目录

环境仅限单用户使用

termux使用与安卓操作系统相同的libc和动态链接器

这三个主要的区别会在你运行典型的 GNU / Linux 系统编译程序时发生问题

与Linux的差异
Termux与常规的Linux发行版之间存在一些差异
Termux中的环境设置类似于现代Linux发行版的环境设置。然而，在Android上运行注定它会有几个重要的差异

Termux 不遵循FHS标准
不像大多数Linux发行版，termux不遵循文件层次系统(FHS)

在常规位置你找不到 /bin, /etc, /usr, /tmp 等文件夹因此，所有程序进行了修补和重新编译，以满足Termux环境，否则它们将无法找到它们的配置文件或数据

您可能在执行具有标准 Shebang (例如#!/bin/sh) 的脚本时遇到问题

Shebang 通常出现在类 Unix 系统的脚本中第一行，作为前两个字符。在Shebang之后，可以有一个或数个空白字符，后接解释器的绝对路径，用于指明执行这个脚本文件的解释器

在执行shebang命令之前，可以使用 Termux-Fix-shebang 命令修改这些文件Termux的最新版本提供了一个特殊的包(Termux-exec)，它让使用标准的shebang成为了可能

大多数包都具有依赖项的共享库，这些依赖项被安装到 $PREFIX/lib 中

根文件系统存储为普通应用程序数据
根文件系统和用户主目录位于位于/data分区上的私有应用程序数据目录中。这些目录的路径分别是 $PREFIX，$HOME

您不能将 $PREFIX 移动到其他位置，因为所有程序都希望 $PREFIX 不会更改。此外，您不能在SdCard上使用 $PREFIX 的二进制文件、符号链接和其他文件。原因很简单——Termux文件系统不支持Unix权限、符号链接、套接字等.

重要：如果您卸载应用程序或擦除数据，目录$PREFIX和$HOME 与其下的文件和文件夹也将一并被删除。所以在进行此操作之前，请确保你备份了所有重要数据

Termux 是单用户的
Android应用程序被沙箱化，并拥有自己的Linux用户ID和SELinux标签。Termux 也不例外，Termux中的所有内容都是使用与应用程序本身相同的用户id执行的

我们所有的包(只有root包除外)都被修补以删除任何多用户和其他类似的功能。我们还更改了服务器包的默认端口：ftpd、httpd 和 sshd 的默认端口分别设置为8021、8080和8022（这个端口在ssh服务上用得到）

您可以读写所有应用程序组件，包括$PREFIX

注意: 这意味着你很容易删除或覆盖$PREFIX中的文件而破坏 Termux 系统



新手上路

更换软件源
通过编辑文件来实现个性化配置,是 Unix 的老传统了, Linux也继承了这点

更何况是模拟Linux环境的Termux呢

Termux 作为一款国外的软件，其软件包所在的存储库都是放在国外服务器进行维护与更新的

因为众所周知的原因, 从国内访问国外服务器的网速是很慢的

国内一般用清华大学提供的镜像源提供软件的更新下载

在我的理解中,其原理是这样的:

通过Termux事先配置好的网址，对存储库所在的服务器进行访问，查看有无要更新或想安装的软件包。由于网速不佳,所以数据总是丢失, 或容易造成访问超时,进而更新失败

国内的清华大学有专门的梯子,将那些相应的存储库(也就是Termux软件的存储库)给搬到国内的服务器上,再开放网站给大家下载

如何找到自己要的镜像源呢?可以通过百度清华镜像源,进入其官网所在,搜索关键字(比如在此处就是Termux)

清华的termux镜像源说,要想更换软件包源,可以通过键入下面三条命令完成



sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list

sed -i 's@^\(deb.*games stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list

sed -i 's@^\(deb.*science stable\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list


这三条命令的原理大同小异,我来解读第一条,其他两条读者可以试着分析

sed 是使用 sed 软件, -i 则是sed软件定义好的使用方式,

sed 是对文件进行读写操作的软件

这里sed 对什么进行了什么操作呢? 看命令的末尾

$PREFIX/etc/apt/sources.list

这便是sed 的操作对象，按termux文件系统规定，$PREFIX 是放包文件的地方, /etc 是放配置， /apt 是放有关软件包的地方

而事实上, 该文件(sources.list) 其实就是存有软件源网站的文件

这语句其实就是用 sed 软件在这个文件中找到一个特定的字符串进行操作

而这里的操作是将这个字符串,其实是将事先配置好的官方源网站,给注释掉

然后再加上一条国内镜像源的网站，即

https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24

这样,就更换好了国内源, 最后来一句指令

apt update && apt upgrade

对其进行加载与更新

（sed的具体详情与使用可以查阅百度）

还有其他教程吗
安装man, info 软件

善用这两个软件包能帮助你解决一些问题，具体详情及用法请百度

用命令行安装软件时＋个 -y 参数

默认安装,不询问

你可以通过下面链接进行进一步的学习

学习 Linux 命令发现更多Linux命令

https://linuxjourney.com/

学习 shellShell脚本是使用终端的一个重要部分，这些是shell(Bash)脚本的指南

http://mywiki.wooledge.org/BashGuidehttps://www.tldp.org/LDP/Bash-Beginners-Guide/html/


用户界面
termux提供的是一个终端界面，其文本大小可以通过手指对屏幕的缩放或拉动调整内容除了终端，还有三个额外的界面元素可用：菜单、导航抽屉和通知

菜单可以通过长按终端上的任何地方来显示，菜单条目提供：

选择和粘贴文本

重置终端

退出当前终端会话

通过选择字体和配色对终端进行造型

显示这个帮助页面

在屏幕左侧向右滑动可以显示导航抽屉，如果你在安卓中启用了手势导航,你需要在滑动之前短暂地按住屏幕边缘以打开导航抽屉，否则会返回，它有三个元素：

点击一个会话让它在终端显示

长按允许您指定一个会话标题，长按命名会话

显示或隐藏扩展键

创建新终端会话的按钮

当终端会话在运行时，可以通过下拉通知菜单

看到Termux通知，点击该通知将打开当前的终端会话，通知也可以展开三个选项

退出所有正在运行的终端会话

使用唤醒锁避免进入睡眠模式

使用高性能WiFi锁来最大化WiFi性能

通过唤醒锁，即使没有终端会话运行，Termux的通知和后台进程也可以使用，这使得服务器和其他后台进程运行得更稳定（比如使用ssh远程访问的时候）


终端设置

普通设置
Termux 终端可以通过在 $HOME/.termux 目录下创建文件 termux.properties 进行配置此文件使用简单的 key=value 属性语法,并允许配置下面列出的属性

更改属性文件后,更改将通过执行 termux-reload-settings 或重新启动 Termux 应用程序(关闭所有会话并重新启动应用程序)生效

在 Termux 0.66版本之后，可以通过 $HOME/.termux/termux.properties 自定义扩展键排

注意：可以配置文件不代表这个文件事先就存在的 有时需要你自己按照相关说明去新建并编辑同名文件, 至于说明在哪，一般在官网帮助处

终端会话处理
可以设置快捷键组合来处理常见的会话操作。打开新会话、在会话之间切换和重命名会话可以调整如下：

打开一个新的终端ctrl + t (音量下键+ t)shortcut.create-session = ctrl + t

切换至下个终端 (例如) ctrl + 2shortcut.next-session = ctrl + 2

切换至上一终端 (例如) ctrl + 1shortcut.previous-session = ctrl + 1

终端重命名 (例如) ctrl + nshortcut.rename-session = ctrl + n

黑色主题
您可以通过配置use-black-ui = true

如果系统用户界面使用黑暗（夜间）主题，则在Android 9及更高版本上自动启用此功能

响铃字符处理
设备振动(默认)

    bell-character=vibrate

响铃

bell-character=beep

无反馈

bell-character=ignore

返回键处理
在按下返回键时可以设置返回键属性的配置如下：

按下esc键

back-key=escape

隐藏键盘或退出app(默认)

back-key=back

扩展键处理
Alt、Ctrl、ESC等按键的使用对于操作CLI终端是必要的

Termux触摸键盘并没有这些按键，为此，Termux使用音量下键按钮来模拟Ctrl键

例如，在手机键盘上同时按下音量下键+L与在硬件键盘上同时按Ctrl+L是发送相同的输入。将Ctrl与键结合使用的效果是什么取决于所使用的程序，但对于绝大多数命令行工具，以下快捷方式是一样的效果：

Ctrl+A将光标移至行首

Ctrl+C阻止当前进程

Ctrl+E光标移至行尾

Ctrl+U从行首删至光标处

Ctrl+K从光标处删至行尾

Ctrl+L清屏

Ctrl+Z暂停当前进程

Ctrl+W向后删除一单词

Ctrl+alt＋c打开新的终端，只在黑客键盘上生效

音量上键搭配其他按键时能提供特定的特殊按键

Volume Up+E → Escape 键

Volume Up+T → Tab 键

Volume Up+1 → F1 (and Volume Up+2 → F2, etc)

Volume Up+0 → F10

Volume Up+B → Alt+B, 移至上一个单词

Volume Up+F → Alt+F, 移至下一个单词

Volume Up+X → Alt+X

Volume Up+W → 上箭头

Volume Up+A → 左箭头

Volume Up+S → 下箭头

Volume Up+D → 右箭头

Volume Up+L → |

Volume Up+H → ~

Volume Up+U → _

Volume Up+P → Page Up

Volume Up+N → Page Down

Volume Up+. → Ctrl+\ (SIGQUIT)

Volume Up+V → 控制音量

Volume Up+Q → 显示或隐藏扩展键

Volume Up+K → 同上

扩展键
Termux还有一个扩展键排以扩展当前的键盘

要启用额外的键视图，您必须长按抽屉中的KEYBROAD按钮——也可以通过同时按音量上键＋Q键或音量上键＋K键

在Termuxv0.66版本之后，扩展键排可以通过创建或编辑文件~/.termux/termux.properties 进行配置

集成开发环境
Termux是一个很棒的软件开发环境下面介绍如何使用以下编程环境之一实现对软件的开发

包管理
Termux 使用 apt 和 dpkg 进行包管理，类似于Ubuntu或Debian

不过在Termux中更推荐使用的是pkg包管理工具，pkg 包管理工具是对apt的封装。

它可以通过自动更新apt列表来简化软件包的安装或升级，这样您就不必在安装或升级包时还需键入  apt update  进行更新

安装包

pkg install package

卸载包

pkg uninstall package

列出所有包

pkg list

升级所有包

pkg upgrade

重要事项：在安装任何东西之前，确保所有软件包都是最新的。此外，我们建议每周至少检查一次更新。我们使用滚动发布更新风格，并且不关心当前版本和以前版本之间兼容性如何。如果您已经很长时间没有升级Termux安装包，又在运行包管理器时遇到错误，那么最简单的修复方法就是完成对所有包的升级并重新安装你想要安装的包。

有关可用命令的更多信息，您可以只输入不带参数的pkg 或 pkg help 命令

C
软件名: clang

描述: LLVM 编译器 

官网: http://clang.llvm.org/

pkg install clang

注意: 它不是 gcc 编译器，LLVM编译器与Gcc编译器实现是有点差异的，Termux貌似不提供 gcc 编译器，但在使用命令行进行编译C语言程序时，可以采用 
gcc file.c ，为什么？历史渊源，详情百度。

Python
Python是一种解释性的、高级的、通用的编程语言。Python由 Guido van Rossum创建，于1991年首次发布。它的设计理念强调代码的可读性，并显著使用了重要的空格。它的语言构造和面向对象的方法，旨在帮助程序员为小型和大型项目，编写清晰、逻辑的代码。

在Termux中，安装Python可以通过命令 pkg install python

旧式版本2.7.x则是 pkg install python2

Python包管理
安装Python后，pip(pip2——如果使用python 2)包管理器也可以使用。下面介绍关于如何使用它的快速教程。

安装一个新的模块

pip install module

卸载一个Python模块

pip uninstall module

列出所有已安装的模块

pip list

在安装Python模块时，强烈建议安装包构建(这是必不可少的，一般它问你安装xx吗? 就直接输入y好了)，一些模块在安装过程中会编译设备的扩展

因为在安装 python 模块时是用 pip这一管理包软件工具

同 Termux的软件源是通过国外的服务器，pip 也有会有网速崩坏的风险

所以需要对pip换源

源文件文件在~/.pip/pip.conf

pip 换源
临时修改
可以在使用pip的时候加参数-i https://pypi.tuna.tsinghua.edu.cn/simple

例如：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple gevent

这样就会从清华这边的镜像去安装gevent库。

永久修改
修改 ~/.pip/pip.conf (没有就创建一个)， 编辑内容如下：

[global]
index-url =http://pypi.douban.com/simple  
[install]
trusted-host=pypi.douban.com


Git
版本控制软件，强烈推荐官网教程《Progit》（有中文版）进行学习，比看视频强多了，安装命令如下：

pkg install git



Vim
包名: vim

主页: https://www.vim.org

一个允许用户编辑文本的系统或程序。文本编辑器是用于编辑纯文本文件的一种程序类型。可用于编辑，更改配置文件、文档文件和编程语言的源代码。

在执行 pkg install vim 安装vim 之后，在vim中键入以下命令:help来了解有关VIM的常见问题

如果你想学习如何使用 Vim，可以在终端下输入 vimtutor







Shell

shell是命令语言解释器，其执行来自标准输入设备（如键盘）或文件的命令

shell不是系统内核的一部分，但可以使用系统内核执行程序，创建文件等功能

能通过Termux-Tools使用 chsh 命令更改登录的shell

目前Termux支持Bash，Fish，Tcsh，Zsh（推荐）和其他一些shell。

BASH
主页: https://www.gnu.org/software/bash/

Bash 是安装termux后默认的shell

BASH shell 初始化文件在  ~/.bashrc, $PREFIX/etc/bash.bashrc 

使用 man bash 或 info bash 查看更多信息

ZSH
主页: https://www.zsh.org/

安装命令: pkg install zsh

Zsh 是一个专为交互式使用而设计的shell，但它也是一种强大的脚本语言。Bash，Ksh 和 Tcsh 的许多有用功能被纳入 Zsh

Zsh shell 配置文件在 ~/.zshrc 和 $PREFIX/etc/zshrc


使用 man zsh 或 info zsh 查看更多信息



SSH 远程访问
在 Termux 通过命令

pkg install openssh

安装 ssh 软件，该软件实现电脑连接手机上的termux，需要电脑也有（一般电脑都有）ssh 软件，保持手机，电脑是同一WiFi

接着输入命令启动ssh服务

sshd



输入命令设置密码

passwd

然后在termux终端上输入两条命令

ifconfig ，whoami

获取手机IP与 Termux终端的用户名

在电脑命令行输入

ssh UserName@YourIp -p 8022

便能用电脑访问 Termux 啦


存储
主要的存储类型
Termux 自身的存储空间

存放在 $HOME ，$PREFIX 中

共享内部存储：设备中供所有应用程序使用的存储空间

在Android 6.0以上的版本，这需要用户授予Termux访问权限, 授予后通过 $HOME/storage 进行访问要访问共享内部存储，您需要执行命令termux-setup-storage

然后系统会提示您“允许Termux访问设备上的照片、 媒体和文件”，您应该允许这样做，允许termux读取存储权限可确保：

在Android 6.0或更高版本上运行时，你能用termux读取或写入内部存储，创建外部存储上的应用程序专用文件夹（如果存在外部存储），创建文件夹 $HOME/storage，该文件夹指向了共享内部存储的软链接

你也可以通过使用 ln 软件实现软链接，什么是软链接，作用是什么，就请读者自己百度学习吧（因为我也不是很懂）。

内外存储的各文件夹简介
诸如shared，download等文件夹，一般都是安卓手机事先设置好的文件夹，由各个厂商按照定义来使用，Termux神器之处在于它提供了软链接，storage，来放着这些标准目录，让用户能更方便的对手机上的文件进行操作，别忘了，Termux上可是有提供大量软件的

$HOME/storage

存放以下目录的文件夹

$HOME/storage/shared

访问共享内部存储的根目录

$HOME/storage/downloads

系统浏览器下载文件所在的标准目录

$HOME/storage/dcim

照相机所产出图片和视频所在的标准目录

$HOME/storage/pictures

用于放置用户可用图片的标准目录

$HOME/storage/music

用于放置用户可用音频文件的标准目录

$HOME/storage/movies

用于放置用户可用影视资源的标准目录

用文件管理器访问Termux

您可以使用文件管理器访问Termux主目录（$HOME）

个人总结
Termux 作为一款安卓上仿Linux环境的应用，我认为其最大作用在于手机这一工具所赋予的便利性和可移动性。私以为，受限于屏幕和键盘，你很难把它作为一款学习工具，但无疑地，它的特点让它成为你能随地温习C语言，或Python知识的有力工具，如果你想熟悉Linux的命令，或是其平台上软件的使用，它也是个不二法门。

初学者在初学Termux时，应按照这样的流程熟悉，运用该软件：从个性化，如更换软件源，编辑扩展键盘，调度字体和配色的样式，安装自己所需要的软件，再到学习软件，命令行的使用，搭建termux的远程访问，熟悉termux的框架，安排，最后在i+1的基础上广泛阅读他人的教程（推荐国光termux教程，有能力再看官网的英文教程），结合自身的创造性，发现使用termux的无限可能。




