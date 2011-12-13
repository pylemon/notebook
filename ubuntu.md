### 常用命令 ###

#### scp ####
	
将远端服务器上的文件下载到本地
	
	$ scp liwei@steve:/home/liwei/red5-1.0.0-RC1.tar.gz ./
	liwei@steve's password: 
	red5-1.0.0-RC1.tar.gz                         100%   40MB  13.3MB/s   00:03  

scp username@servername:/dir/filename ./file


### 编译安装 ###

#### configure errors ####

1. *./configure* 提示 `No package 'zlib' found`      
	`$ sudo apt-get install zlib1g-dev`

2. *./configure* 提示 `No package 'gtk+-2.0' found`    
	`$ sudo apt-get install libgtk2.0-dev`
