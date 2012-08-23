==========
 常用命令
==========

scp
===

scp用来在服务器和本地之间复制文件非常的方便

将远端服务器上的文件下载到本地 ::

    $ scp liwei@steve:/home/liwei/red5-1.0.0-RC1.tar.gz .
    liwei@steve's password:
    red5-1.0.0-RC1.tar.gz                         100%   40MB  13.3MB/s   00:03

* scp 的常用使用格式 ::

    $ scp 用户名@主机名(IP):远端路径 本地路径
    $ scp 本地路径 用户名@主机名(IP):远端路径

和 cp 一样，前面的是源文件，后面的是目标文件。
