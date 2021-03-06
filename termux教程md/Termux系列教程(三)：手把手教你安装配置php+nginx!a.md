05.01 18:01
Termux系列教程(三)：手把手教你安装配置php+nginx！
Termux系列教程(三)：手把手教你安装配置php+nginx！
原创 bcdroid 手机编程 2020-10-07
大家好，这里是 「手机编程」，我是作者：舞剑，记得「关注我」

今天是Termux系列第三节，我来讲讲怎么安装 PHP 与 Mysql，然后用 Termux 搭建一个网站。


PHP
全球有几乎95%的网站都使用 php 需要编写的，尤其是 Wordpress 这款开源框架，简直是万金油，博客可以用它，淘宝客可以用它，官网也可以用它……，简直是中小企业与个人建站必备良药。

安装PHP

Termux 封装了 php，所以安装很简单，只需要一行 pkg 命令就行了。

pkg install php

与 Python 安装一样，会提示“需要下载21.6m的包，并且要使用107m的手机存储，是否继续？”，直接输入 Y 确认即可。



网速好的话，稍等片刻，很快就安装成功了。

查看php版本

php 一直更新，许多软件也要求新版本，查看版本代码：

php --version

我这里安装的 php 版本为 7.4.10



关于这一点 Termux 软件源会一直更新，可能你安装的和我不同，都是最新版，不用在意。


Nginx
Nginx 是一款高性能 web 服务器，常常与 php 配合解析并搭建网站，要使用它，只需要两个步骤：安装与配合。

安装 Nginx

在这一点上，Nginx 的安装与大多数软件一样，只需要输入代码：

pkg install nginx

网速好的话，只需要 15s 左右就能安装成功！


▌查看 nginx 版本
nginx -v
nginx version: nginx/1.19.2

版本号略过不说，继续说说如何启动 nginx。

▌启动Nginx
nginx 的启动很简单，只需要在终端输入代码：

nginx

怎么看是不是真的启动了呢？

①用 pgrep 命令查看终端号


②访问本地链接

Nginx 服务器的默认端口是 8080，那么，只需要在手机浏览器输入：127.0.0.1:8080


从网页返回的内容可以看出 nginx 正常启动。


Nginx解析PHP
nginx 与 php 都安装好了，接下来就要把这两个程序给链接在一起。

nginx 与 php 是单独的两个程序，运行起来也是各干各的，互不相干。要链接它们，就需要用到 PHP-FPM。

PHP-FPM

PHP-FPM 是什么？

简单说，nginx 是一个静态 web 服务器，只能解析处理 html 这类静态文件，对于 php 这种动态语言无能为力，所以要把 php 请求交给 php解释器 处理，可怎么才能区分 html 与 php 文件呢？

nginx 在察觉到 php 文件时，该怎么把它交给 php解释器 呢？

猜对了，就是用 PHP-FPM。

关于这一点，大家前期只需要知道 php-fpm 是 nginx 与 php 之间的互动桥梁就行了，更深入的内容留在后面讲。

▌安装 php-fpm
Termux 终端输入命令：

pkg install php-fpm

文件不大，10s 左右就安装好了。

▌配置 php-pfm
安装的 php-pfm 配置文件在绝对路径 /data/data/com.termux/files/usr/etc/php-fpm.d/www.conf ，使用如下命令：

vim $PREFIX/etc/php-fpm.d/www.conf

用文本编辑神器 vim 打开 www.conf 文件，修改如下代码：

listen = /data/data/com.termux/files/usr/var/run/php-fpm.sock

更改为：

listen = 127.0.0.1:9000

ok，完工，php-pfm修改完毕，是不是很简单，接下来配置 nginx。


配置 Nginx

Nginx 的配置文件在绝对目录 /data/data/com.termux/files/usr/etc/nginx/nginx.conf， 用 vim 打开 nginx.conf 文件：

vim $PREFIX/etc/nginx/nginx.conf

在这个文件中，总共需要修改三处。

▌第一处
第一次处是让 nginx 识别出 php。



找到第 45 行，在结尾追加 index.php，这样一来，nginx 就能识别出默认 index.php 主页。


▌第二处
第二处，在 65-71 行之间，把 # 号删除掉，然后继续修改第 69 行，原内容为：

fastcgi_param  SCRIPT_FILENAME /scripts$fastcgi_script_name;

在这里，/scripts 代表的是网站的根目录，我的手机没root，如果想新建网站，只能用 vim 编辑器在根目录编写 php 代码，对新手太不友好了。

不如在文件管理器新建一个 nginx 文件夹，然后把它 /scripts替换为 /data/data/com.termux/files/home/storage/shared/nginx。

修改后的代码如下所示：

fastcgi_param  SCRIPT_FILENAME /data/data/com.termux/files/home/storage/shared/nginx$fastcgi_script_name;

这样一来，直接就能用手机自带的文件管理器来管理和编写 php 代码，爽翻了。



▌第三处
光顾着修改 php$ ，差点忘了 /根目录 ，上滑回到第一处的位置，也就是第 44 行，修改 root(根) 内容为 /data/data/com.termux/files/home/storage/shared/nginx。


与第二处原理一样，把网站放在了手机存储中。

来测试一下配置有没有生效。

测试
打开文本编辑器，我用的是 QuickEdit，在 storage/nginx/ 目录下新建一个文件 info.php， 输入如下代码：



这行代码主要用来查看服务器的主机信息，也是开发者测试代码。


启动服务

启动 php-fpm

php-fpm

启动 nginx

nginx

访问 info.php

打开浏览器，输入 127.0.0.1:8080/info.php ，出现如下页面：




很好，配置生效了，nginx 正常解析 php，


后记
其实，照着步骤一步一步操作，没什么太大的难度。

为了写这篇文章，我特地卸载重装了 Termux ，每一个步骤都经过了验证，排版切图写文共花了8个小时才写完，希望能帮助到大家。

对了，下一节讲一讲安装 mysql，顺便分享并搭建一套免费的vip影视源码。


ps:文中主要安装教程参考自@国光大佬

记得关注我，持续更新！



推荐阅读: 

① 只用手机，我学会了编程！

② 手机编程，该用什么输入法？

③ 手机端最强编程APP，开箱即用，功能强大！

④ 吊打AIDE的Java开发环境，支持Maven、JDK11，速进！

⑤ 手机编写C语言神器，集成gcc插件，可导出为APK！

⑥ 适合新手的Java开发环境，简化Maven流程，傻瓜式下载Jar包。国产威武！！！

⑦ 手机随时随地写Python，还可以开发安卓APP，太厉害了！

⑧ 吊打QPython的集成开发环境，无广告，无BUG，已完美解锁！



关于我
作者：舞剑，专注手机编程的程序猿！

我是舞剑，舞刀弄剑的那个舞剑。我不是专业程序猿，只是一心热爱、探究这个神秘的领域！

在我看来，程序员就像古代的巫师，只需要按下神秘的代码，奇迹就会发生。


微信公众号：
手机编程

知乎号/简书号/掘金号：
手机编程

网站
手机编程： https://www.bcdroid.com/



Termux系列教程


php8.0


修改   www.conf    36——61行



修改  nginx.conf    45——39行



修改  nginx.conf    69——9行



修改  nginx.conf    44——68行







