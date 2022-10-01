### https和git区别

https clone不需要密码，push需要用户名和密码

git需要配置密钥



### 在本地仓库添加远程映射

git remote add 远程仓库别名 远程仓库地址



### 从其他地方复制的公私钥不起作用

更改权限为其他人无法操作 700

ssh-add 私钥文件

# git使用

初始化
git init
git config --global user.name=''
git config --global user.email=''

创建合并分支
git checkout -b 分支名
