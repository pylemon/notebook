## Ubuntu Linux 安装
### Linux文件系统一

文件系统是操作系统用于明确磁盘或分区上文件的方法和数据结构，即在磁盘上组织文件的方法。
比如大家熟知的Ｗindows下的文件系统 FAT32 和 NTFS

Linux最常见支持的文件系统有EXT2/EXT3/ETX4等

EXT2
ext2是可扩展的高性能的文件系统。在2000年前是Linux/GNU的标准文件系统，
可以支持256字节的长文件名，单一文件大小上限为2048GB, 而文件系统的理论容量上限为6384GB。

EXT3
ext3是一种日志式文件系统，日志文件系统可以在系统发生断电或者其它系统故障时保证整体数据的完整性，
ext3在ext2的基础上加入了记录元数据的日志功能。 ext3 目前所支持的最大 2TB 的单一文件 理论容量上限为 16TB

EXT4
Linux 内核自 2.6.28 开始正式支持新的文件系统 ext4, 
目前最新的 Ubuntu 11.10 的内核已经升级到3.0
安装系统时推荐的文件系统也是 ext4
EXT4的几个强大之处:
1.向上兼容EXT3, 只需要几个命令就能从ext3升级到ext4,无需重装系统。
2.支持更大的文件和文件系统(16TB单文件，1EB文件系统[1,048,576TB， 1EB=1024PB， 1PB=1024TB])
3.无限数量的子目录。Ext3 目前只支持 32,000 个子目录，而 Ext4 支持无限数量的子目录。 
...

其他的文件系统还有：ReiserFs, XFS


### Linux文件系统二

常用目录作用

/      根目录
/bin   用户命令的可执行文件 
/dev   特殊设备文件 
/etc   系统执行文件、配置文件、管理文件，主要是配置文件 
/home  用户目录
/lib   引导系统以及在root文件系统中运行命令所需的共享库
/mnt   临时挂载（mount）的文件系统（如光驱、软驱） 
/sbin  只有root使用的可执行文件和只需要引导或安装/usr的文件
/tmp   临时文件 
/usr   为用户和系统命令使用的可执行文件、头文件、共享库、帮助文件、本地程序（在/usr/local中） 
/var   这个目录中存放着那些不断在扩充着的东西，如日志，邮件。
/opt   附加的应用软件包，通常大型第三方软件的安装目录

分区
1.boot
存放Linux的启动引导文件 100M 就足够了

2./ （有些linux发行版，这个也写做：/root，功能道理是一样的）
根目录，存放系统相关的文件。至少5G以上。

3./swap
交换分区，类似与windows的虚拟内存，但又不完全一样。有些软件会依赖这个分区，不分会导致某些软件无法运行。
建议与内存大小一致即可

4./home
上面提到过home存放的是用户的文件和个性化配置。这个分区尽量分的大一点。建议30G以上

分区的格式可以都采用ext4，swap分区有专门的格式

### 系统安装
wubi安装
硬盘安装



## 常用的shell命令
### shell - 基本命令

基本命令：

* ls
显示文件列表：ls [-options] [filelist]
如果不指定filelist参数，则列出当前目录中的所有文件； filelist参数既可以是绝对路径也可以是相对路径
不带任何选项的ls命令只列出文件名
-a 用于列出目录中的所有文件，包括文件名以“.”开头的隐藏文件
-l 以长格式列出文件的详细信息：文件的类型、操作权限、链接数、属主名、属组名、字节数以及最近修改时间

* cat head tail more less
显示文件内容的命令：cat head tail more less [filelist]
后面的参数为待显示的文件列表
cat  显示整个文件的内容
head 显示文件头
tail 显示文件尾
more 能够在终端上逐页地显示一个或多个文件，在每屏的最后一行给出提示，显示目前显示了百分之多少的内容，
并可根据不同的输入命令继续显示后续的文本内容
less 浏览文件内容，向上向下翻页使用 j k， q退出

目录操作：
pwd	显示当前路径
cd	改变当前工作目录（不带参数时回到home目录）
mkdir	创建目录
rmdir	删除目录
** 当使用rmdir命令时，要求被删除的目录是个空目录，否则必须使用-r选项


### shell - 文件操作

文件操作：

Copy文件：
cp [-options] 源文件列表 目标文件列表

移动文件（改名）：
mv 源文件列表 目标文件列表

删除文件：
rm 文件列表

创建空文件：
touch 文件名

创建文件链接：
ln –s 链接文件 源文件

查看文件大小：
du -h 文件或目录名
-h human_readable方便阅读的文件大小

常用参数：
-r 目录递归
-f 强制
-I 交互


### shell - 文件权限

设置文件/目录操作权限：

stat 文件名
查看文件信息

```shell
$ stat settings.py
  File: "settings.py"
  Size: 6623      	Blocks: 16         IO Block: 4096   普通文件
Device: 815h/2069d	Inode: 533104      Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/  liwei2)   Gid: ( 1000/  liwei2)
Access: 2011-12-21 13:56:32.028138934 +0800
Modify: 2011-12-15 12:22:41.480758174 +0800
Change: 2011-12-15 12:22:41.480758174 +0800
```
Access中的(0664/-rw-rw-r--)为文件权限信息 使用 ls -l 也可以看到
权限长度为10个字符组成，除第一位外其他9位分为三组 分别表示 文件的属主 同组的成员 其他用户，
第一位表示文件的类型
- 普通文件
d 文件夹
l 符号链接
b 块设备文件
c 字符设备文件
s 套接文件
p 管道文件

后三组中 每组按照rwx的顺序为一组（rwx的顺序不可逆） r表示读取或复制 w表示写 x表示运行
**需要注意 目录可以看作是一种特殊类型的文件，只有当用户拥有该文件的执行权限时才可以进入目录

改变文件的权限
基本语法：chmod [-options] mode files

mode为设定的权限，有两种设定方式：符号方式和八进制数值方式
符号方式的mode格式为：user   operator   access

user表示用户的分类： operator表示设置运算符：  access表示权限类型： 
u	文件的属主         + 添加                  r 允许读或复制一个文件
g	同组的成员         - 清除                  w 允许写一个文件
o	其他用户                                  x 允许运行一个可执行文件
如：chmod u＋x,o-x file 或 chmod uo-x file

*另外还可以用八进制来表示文件的权限，每三位字符看作一个二进制数
rw-  对应  110(bin)  6(dec)
rwx  对应  111(bin)  7(dec)
常用的 chmod 777 file
将file的权限设置为 -rwxrwxrwx

### shell - 压缩备份

文件压缩/备份：
.tar
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）

.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName

.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName

.gz
压缩：gzip –c 文件名 > 文件名.gz
解压：gzip –d xxx

.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName

.rar
解压：rar a FileName.rar
压缩：rar e FileName.rar


### shell - 系统管理

sudo 以管理员身份运行
系统的一种自我保护机制，当提示permission denied时，表示操作可能对系统造成损坏，需要在执行的命令之前加上sudo

软件安装
更新源
sudo apt-get update
更新系统
sudo apt-get upgrade
搜索应用
apt-cache search 应用名称
安装应用
sudo apt-get install 应用名称
删除应用
sudo apt-get remove 应用名称


用户管理
useradd 增加用户
userdel 删除用户
passwd 修改用户口令

网络管理
ifconfig 查看端口配置(ip地址 mac地址等信息)
iwconfig 查看无线网络配置
ping 测试连通性，使用 Ctrl-c 停止


文本编辑
vi是所有UNIX系统都有的一个文本编辑器,工作在两种模式：
命令模式：初进入vi，在编辑模式下按’Esc键
编辑模式：在命令模式下按“iIaAoO”等键进入编辑模式
常用的vi命令：vi filename :打开或新建文件，并将光标置于第一行首。
常用命令 q 退出 w保存 !强制
可以组合使用例如 wq!保存并退出

内存使用
free用来显示内存的使用情况，使用权限是所有用户
free [－b|－k|－m] [－o] [－s delay] [－t] [－V]
－b －k －m：分别以字节（KB、MB）为单位显示内存使用情况。
－s delay：显示每隔多少秒数来显示一次内存使用情况。
－t：显示内存总和列。
－o：不显示缓冲区调节列。
例如:
free –m –s5


进程管理
进程是Linux系统中一个非常重要的概念。Linux是一个多任务的操作系统，系统上经常同时运行着多个进程。
我们不关心这些进程究竟是如何分配的，或者是内核如何管理分配时间片的，所关心的是如何去检视和控制这些进程，
让它们能够很好地为用户服务。
ps 生成进程的列表 
a  显示现行终端机下的所有程序，包括其他用户的程序。
u  以用户为主的格式来显示程序状况。 
x  显示所有程序，不以终端机来区分。
-A 显示所有程序。
c  列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示。 
-e 此参数的效果和指定"A"参数相同。 
e  列出程序时，显示每个程序所使用的环境变量。 
f  用ASCII字符显示树状结构，表达程序间的相互关系。 
-N 显示所有的程序，除了执行ps指令终端机下的程序之外。 
s  采用程序信号的格式显示程序状况。 
S  列出程序时，包括已中断的子程序资料。  
最常用的方法是ps -aux , 然后再利用一个管道符号导向到grep去查找特定的进程,然后再对特定的进程进行操作.
ps -aux | grep python

Kill 向内核发送一个系统操作信号和某个程序的进程标识号，然后系统内核对进程标识号指定的进程进行操作。
$ kill PID 
强制终止指定进程号(PID)的进程
$ kill –9 PID 
强制终止指定进程号(PID)的进程



## 系统配置
### 配置邮箱
...
### 安装打印机
...

## 项目相关

### 版本控制
目前项目正在使用的版本控制工具是subversion
图形化界面的svn工具RabbitVCS基本保留了TortoiseSVN的使用习惯
http://rabbitvcs.org/

命令行svn工具 subversion

1.安装
sudo apt-get install subversion

2.检出文件（checkout）。
使用命令：svn co 项目地址 /目标文件夹
如：svn co svn://172.16.16.39/adaptive/trunk
输入用户名和密码后 会检出项目到当前目录下

3.提交文件（commit）。
进入需要更新的目录，输入命令：
svn commit -m ‘svn提交日志’

4.更新文件（update）。
svn update，在要更新的目录运行这个命令就可以了。

5.查看日志（log）。
svn log path


### mysql 数据库

安装mysql
sudo apt-get install mysql-server mysql-client

图形化管理工具 mysql-query-browser

安装
sudo apt-get install mysql-query-browser 

配置
connection name: adaptive
user name: adaptive
password : adaptive
hostname : localhost
