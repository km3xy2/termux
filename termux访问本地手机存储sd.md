04.30 06:55
termux访问手机存储sd
开启本地访问权限

termux-setup-storage

$ ls storage/shared
0a0fb490a13cc953  baidu
Alarms            binary.bin
Android           cache
AppClone          com.iqoo.secure_TMF_TMS
Backup            com.quark.browser
BaiduMapSDKNew    com.tencent.mobileqq
Catfish           com.tencent.mtt
DCIM              com.tencent.qqlive
Download          com.vivo.browser_TMF_TMS
MasterArchive     grab
Movies            qt
Music             sogou
Notifications     tad
Pictures          tbs
Podcasts          tbslog
QQBrowser         tencent
QTAudioEngine     ttscache
Quark             umeng_cache
Ringtones         vipc
alipay            vivo
attribution       weishi_yt_model
autonavi          下载
backups           阅图锁屏


~ $ cd
~ $ ls
storage  www  www2
~ $ ls www
1.txt  index.php
~ $ ls termux
ls: cannot access 'termux': No such file or directory
~ $ ls storage
dcim       movies  pictures  www
downloads  music   shared
~ $ ls storage/www
index.php  index2.php
~ $ ls storage/shared
0a0fb490a13cc953  baidu
Alarms            binary.bin
Android           cache
AppClone          com.iqoo.secure_TMF_TMS
Backup            com.quark.browser
BaiduMapSDKNew    com.tencent.mobileqq
Catfish           com.tencent.mtt
DCIM              com.tencent.qqlive
Download          com.vivo.browser_TMF_TMS
MasterArchive     grab
Movies            qt
Music             sogou
Notifications     tad
Pictures          tbs
Podcasts          tbslog
QQBrowser         tencent
QTAudioEngine     ttscache
Quark             umeng_cache
Ringtones         vipc
alipay            vivo
attribution       weishi_yt_model
autonavi          下载
backups           阅图锁屏
~ $ ls
storage  www  www2
~ $ ls storage/
dcim       movies  pictures  www
downloads  music   shared
~ $ ls storage/pitures
ls: cannot access 'storage/pitures': No such file or directory
~ $ ls storage/pictures/
WeiXin
~ $ ls storage/pictures/weixin/
wx_camera_1619571327333.jpg
~ $ ls storage/downloads/
 com.tencent.qqlive  'i Theme'   upgrade   www3
~ $

~ $ ls
storage  www  www2
~ $ ls www
1.txt  index.php
~ $ vim  www/index.php
~ $ ls storage
dcim       movies  pictures  www
downloads  music   shared
~ $ ls storage/www
index.php  index2.php
~ $ vim storage/www/index.php
~ $ ls -a
.   .bash_history  .viminfo  www
..  .termux        storage   www2
~ $

