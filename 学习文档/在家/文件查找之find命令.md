##文件查找之find命令

从磁盘上遍历文件，因此速度慢、效率低

语法格式：find [路径] [选项] [操作]

-路径：可以为绝对路径，也可以为相对路径

-选项参数对照表：
  -name 根据文件名进行查找
  -perm 根据文件权限进行查找
  -prune 该选项可以排除某些查找目录
  -user 根据文件属主查找
  -group 根据文件属组查找
  -mtime -n|+n 根据文件修改时间查找
  
  -type 根据文件类型查找
  -mindepth n 从n级子目录开始搜索
  -maxdepth n  最多搜索到n级子目录


find命令总结

常用选项：命令可以根据需要叠加使用 -type  -size  -mtime .....

       -name    find /etc -name '*.conf'
       -iname   find . -iname aa  查找当前目录下的文件，忽略大小写
       -type
            find . -type f   查找文件
            find . -type d   查找目录
       -size
            +n   find /etc -size +1M  大于n的文件
            -n   find /etc -size -1M  小于n的文件
       
       -mtime
           +n   find /etc -mtime +3  三天前修改的文件
           -n   find /etc -mtime -3  三天内修改的文件
       
       -mmin
          +n  n分前的文件
          -n  n分钟内的文件
   
-操作：
-print 打印输出，默认选项
-exec  对搜索到的文件进行特定操作，格式为-exec 'command' {} \;
  find /etc -name '*.conf' -exec cp {} ./text \;  复制文件到指定路径下
-逻辑运算符：
  -a 与 默认选项
  -o  或
  -not|！ 非    find /etc -not -name '*.conf'     find /etc ! -name '*.conf'

   
   
