git命令

#git克隆代码
下载代码有另种形式：
一种是http形式，直接进行下载，只能pull无法push；
一种是SSH形式下载，需要验证ssh秘钥，可pull也可push


##查看远端仓库信息，看远端是哪种格式
*ssh格式
$ git remote -v   
origin  git@github.com:znvxxm/myStudy.git (fetch)
origin  git@github.com:znvxxm/myStudy.git (push)

*http格式
$ git remote -v
origin  http://10.45.156.100/ZNV-E/datacenter.git (fetch)
origin  http://10.45.156.100/ZNV-E/datacenter.git (push)



##查看git用户与邮箱
git config --global user.name
git config --global user.email

##设定git用户与邮箱
git config --global user.name 'user_name'
git config --global user.email 'email'

##生成ssh秘钥
ssh-keygen -t rsa  -C "xie930124@163.com"

##将公钥放入远端仓库设置中
##测试是否已经和远端建立ssh免密通信
ssh  -T git@github.com //检查是否和github联通