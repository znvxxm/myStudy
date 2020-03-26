## source /etc/profile 作用
**source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，将文件里面的指令立即生效构成脚本的运行环境 
**/etc/profile为整个Linux系统环境变量文件，里面的变量对整个Linux环境所有用户生效

**export环境变量在shell的生存周期
1.shell里面的变量会随着shell的结束而消失
2.在shell中使用export，则会给此shell设定一个父环境变量，所有由此父shell产生的子shell均可以使用此变量。改变量随父shell结束而消失

**设置环境变量PATH（export PATH）
1.“/bin”、“/sbin”、“/usr/bin”、“/usr/sbin”、“/usr/local/bin”等路径已经在系统环境变量中了，
如果可执行文件在这几个标准位置，在终端命令行输入该软件可执行文件的文件名和参数(如果需要参数)，回车即可。说明与JAVA_HOME这个变量无关
2.如果不在标准位置，文件名前面需要加上完整的路径。不过每次都这样跑就太麻烦了，一个“一劳永逸”的办法是把这个路径加入环境变量。
命令 “PATH=$PATH:路径”可以把这个路径加入环境变量，但是退出这个命令行就失效了。要想永久生效，需要把这行添加到环境变量文件里。
有两个文件可 选：“/etc/profile”和用户主目录下的“.bash_profile”，“/etc/profile”对系统里所有用户都有效，用户主目录下的“.bash_profile”只对这个用户有效。　　

1、直接用export命令：
　　#export PATH=$PATH:/opt/au1200_rm/build_tools/bin
查看是否已经设好，可用命令export查看：
　　　　[root@localhost bin]# export
　　　　declare -x BASH_ENV="/root/.bashrc"
　　　　................
　　　　declare -x PATH="/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/X11R6/bin:/root/bin:/opt/au1200_rm/build_tools/bin"
可以看到，环境变量已经设好，PATH里面已经有了我要加的编译器的路径。

2、修改profile文件： 
　　#vi /etc/profile 
在里面加入:
　　export PATH="$PATH:/opt/au1200_rm/build_tools/bin"

3. 修改.bashrc文件：
　　# vi /root/.bashrc
在里面加入：
　　export PATH="$PATH:/opt/au1200_rm/build_tools/bin"

后两种方法一般需要重新注销系统才能生效，最后可以通过echo命令测试一下：
　　# echo $PATH
看看输出里面是不是已经有了/my_new_path这个路径了。



##2.寻找文件所在的位置
find /opt -name kafka-console-consumer.sh


##crontab命令使用
https://www.cnblogs.com/runtimeexception/p/10050809.html

crontab -l  //查看该用户下目前的定时任务
crontab -e  //设定定时任务

运行crontab –e 编写一条定时任务 */5 * * * * /home/test.sh 在每5分钟执行一次test.sh脚本

##Linux系统中 /bin、/sbin、/usr/bin、/usr/sbin、/usr/local/bin、/usr/local/sbin 目录的区别
1./bin与/sbin的区别
/bin：    存放所有用户皆可用的系统程序，即普通的基本命令，如：cat，ls，chmod等。
/sbin：   存放超级用户才能使用的系统程序，即基本的系统命令，如：shutdown，reboot等。

2./usr/bin与/usr/sbin的区别
/usr/bin：    存放所有用户都可用的应用程序，一般是已安装软件的运行脚本，如：free、make、wget等。系统升级会重新覆盖，装在里面的程序可直接使用，如Python
/usr/sbin：   存放超级用户才能使用的应用程序 ，一般是与服务器软件程序命令相关的，如：dhcpd、 httpd、samba等。

3./usr/local/bin与/usr/local/sbin的区别
/usr/local/bin：    存放所有用户都可用的与本地机器无关的程序，即第三方软件程序
/usr/local/sbin：   存放超级用户才能使用的与本地机器无关的程序


##/etc/rc.d/rc.local
这个文件中写入了什么命令，在每次系统启动时都会执行一次.
也就是说，如果有任何需要在系统启动时运行的工作，则只需写入 /etc/rc.d/rc.local 配置文件即可.




