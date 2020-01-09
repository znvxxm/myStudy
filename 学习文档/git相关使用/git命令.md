git命令

##查看远端仓库信息
git remote -v

##查看git用户与邮箱
git config --global user.name
git config --global user.email

##设定git用户与邮箱
git config --global user.name 'uwer_name'
git config --global user.email 'email'

##生成ssh秘钥
ssh-keygen -t rsa  -C "xie930124@163.com"

##将公钥放入远端仓库设置中
##测试是否已经和远端建立ssh免密通信
ssh  -T git@github.com //检查是否和github联通