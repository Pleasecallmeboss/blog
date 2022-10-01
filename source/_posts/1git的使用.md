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

创建、合并分支
git checkout -b 分支名

# 配置git自动补全

直接下载 Git 的源代码，进入 contrib/completion 目录，会看到一个 git-completion.bash 文件。
git clone https://github.com/git/git.git
(不推荐直接下载git源码，容量过大；建议直接去github中搜git官方托管库，按以上方法查找到git-completion.bash 文件进行下载，注意下载后文件所放地)
将下载好的该文件复制到你自己的用户主目录中（此处主目录是指你git下的目录，在windows下一般放于git\Git\etc文件下，以下以个人电脑为例，重点重点是找到自己存放git目录）
![](https://gitee.com/Pleasecallmeboss/tuchuang/raw/master/Note/git_location.png)
3.将git-completion.bash文件改成.git-completion.bash。
4.在etc文件夹下找到bash.bashrc文件，打开该文件，在其末尾添加
source .git-completion.bash
键入 git co 然后连按两次 Tab 键，会看到两个相关的建议（命令） commit 和 config。继而输入 m<tab> 会自动完成 git commit 命令的输入。到此处说明你已成功设置好了。

需要在git自带的bash中才可以运行，想在powershell使用将git下的bash.exe添加到环境变量中，然后注销生效，输入bash进入bash环境

