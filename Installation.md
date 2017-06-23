
# Installation

## CentOS 7

Download, extract and compile Redis with: 

	$ wget http://download.redis.io/releases/redis-3.2.9.tar.gz
	$ tar xzf redis-3.2.9.tar.gz
	$ cd redis-3.2.9
	$ make
	The binaries that are now compiled are available in the src directory. Run Redis with:
	$ src/redis-server
	You can interact with Redis using the built-in client:
	$ src/redis-cli
	redis> set foo bar
	OK
	redis> get foo
	"bar"

下载失败，分析得wget未安装，wget软件包安装 

	yum -y install wget

重新下载，下载成功

执行命令make编译出现错误

	cd src && make all
	make[1]: Entering directory `/root/redis-3.2.9/src'
	    CC adlist.o
	/bin/sh: cc: command not found
	make[1]: *** [adlist.o] Error 127
	make[1]: Leaving directory `/root/redis-3.2.9/src'
	make: *** [all] Error 2


分析可得未安装Gcc环境  

    yum install gcc

执行命令make编译出现错误 

	In file included from adlist.c:34:0:
	zmalloc.h:50:31: fatal error: jemalloc/jemalloc.h: No such file or directory
	 #include <jemalloc/jemalloc.h>


原因分析
	在README 有这个一段话。

	Allocator  
	---------  
	 
	Selecting a non-default memory allocator when building Redis is done by setting  
	the `MALLOC` environment variable. Redis is compiled and linked against libc  
	malloc by default, with the exception of jemalloc being the default on Linux  
	systems. This default was picked because jemalloc has proven to have fewer  
	fragmentation problems than libc malloc.  
	 
	To force compiling against libc malloc, use:  
	 
	    % make MALLOC=libc  
	 
	To compile against jemalloc on Mac OS X systems, use:  
	 
	    % make MALLOC=jemalloc

所以执行命令
	make MALLOC=libc

到此安装成功
