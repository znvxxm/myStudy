## source /etc/profile 作用
**source filename：这个命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，将文件里面的指令立即生效构成脚本的运行环境 
**/etc/profile为整个Linux系统环境变量文件，里面的变量对整个Linux环境所有用户生效

**添加环境变量
1.临时变量，指当shell窗口关闭时变量失效，通过export命令添加
export PATH=/usr/local/bin:$PATH
2.全局生效，修改/etc/profile文件，这样环境变量全局生效且不会因窗口的关闭而失效

##2.寻找文件所在的位置
find /opt -name kafka-console-consumer.sh





