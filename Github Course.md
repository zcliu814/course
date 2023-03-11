

# Git&GiHhub&GitHubDesktop教程

[TOC]



## 1. Git常用命令

### 工作流程	

![git工作流程图](D:\OneDrive\桌面\Course\git工作流程图.png)

Workspace：工作区

Index / Stage：暂存区

Repository：仓库区（或本地仓库）

Remote：远程仓库

工作区--->暂存区--->本地仓库--->远程仓库？？？

### 开发分支

```sh
master    :默认开发分支                 Head    :默认开发分支
origin    :默认远程版本库               Head^   :Head的父提交  ^^表示上两个版本
git help                     #查看帮助
git help -a                  #查看所有子命令
git <command-name> -help     ？？？？？？？？？
git help <command?           #查看指定命令帮助
```



### 创建版本库

```shell
git clone <url> [newname]	    #克隆远程版本库(下载),可重新命名
git init [name]                 #初始化本地git仓库（创建新仓库）,命名
git config --list               #显示当前git配置
git config --global user.name "xxx"                       # 配置用户名
git config --global user.email "xxx@xxx.com"              # 配置邮件
ssh-keygen -t rsa -C "xxxxxx.com"                         #创建ssh公钥
```



### 修改和提交

```shell
git status [file-name]          #查看状态
git diff                        #查看变更内容
git diff HEAD^                  #比较与上一个版本的差异
git add .                       #将工作区所有改动提交到暂存区,不包括被删除文件
git add <file1> <file2> ...     #将指定的文件/目录的修改提交到暂存区
git mv <old> <new>              #文件改名
git rm <file>                   #删除文件
git rm --cached <file>          #停止跟踪文件但不删除
git commit -m "commit message"  #将暂存区所有文件添加到本地仓库？？？？？
git commit <file1> <file2> ... -m "commit message" 
                                #提交所有指定的文件添加到本地仓库区
git commit -am "massage"        #将工作区的内容直接加入本地仓库
git commit --amend              #修改最后一次提交，旧的提交会被取消
```



### 查看提交历史

```shell
git log [--all]           #查看提交历史
git log -n          #查看n行日志
git log --stat                                            # 显示提交日志及相关变动文件
git log -p <file>   #查看指定文件的提交历史
git show HEAD                                             # 显示HEAD提交日志
git show HEAD^                                            # 显示HEAD的父（上一个版本）的提交日志 ^^为上两个版本 ^5为上5个版本
git blame <file>    #以列表方式查看指定文件的提交历史
```



### 撤销

```shell
git reset --hard HEAD       #撤消工作目录中所有未提交文件的修改内
git reset --hard xxx        #回滚到指定xxx版本（log查看版本记录）
git checkout HEAD <file>    #撤消指定的未提交文件的修改内容
git revert <commit>         #撤消指定的提交
```



### 分支与标签

```shell
git branch                   #显示所有本地分支
git branch -r                #显示远程分支
git branch -a                #显示所有分支
git checkout <branch/tag>    #切换到指定分支或标签
git checkpoint -b <new-branch>  #创建并切换到新分支
git branch <new-branch>      #创建新分支,但依旧停留在当前分支
git branch -d <branch>       #删除本地分支
git tag                      #列出所有本地标签
git tag <tagname>            #基于最新提交创建标签
git tag -a v1.0 xxx          #-a参数会允许你添加一些信息
git tag- d <tagname>         #删除标签
```



### 合并与衍合

```sh
git merge <branch>     #合并指定分支到当前分支
git rebase <branch>    #衍合指定分支到当前分支
```



### 远程操作

```sh
git remote <-v>                #查看远程版本库信息
git remote show <remote>       #查看指定远程版本库信息
git remote rm <remote>         #删除远程仓库（解除与远程仓库的关联）
git remote rename [old-name] [new-name]         
                               #远程仓库重命名
git remote add <anyname> <url>  #添加远程版本库
git fetch <remote>             #从远程库获取代码的全部改动   -a取到全部
git pull <remote> <branch>     #下载代码及快速合并
git push [-u] <remote> <branch>     #上传代码及快速合并 -u下次直接git push就可以git
                                    #push的时候不包含tag,如果想包含,可以加上--tags参数
git push <remote> :<branch/tag-name>   #删除远程分支或标签#上传所有标签
git push <remote> --all                #推送所有分支到远程仓库
git push --tags                        #上传所有标签
"git pull == git fetch + git merge"
```



## 2.上传项目

### 首次配置

检查本机公钥

```sh
cd ~/.ssh                                   #提示no such file or directory,则是第一次使用git,下一步直接生成就好
```

如果不是第一次使用，先清理原来的ssh密钥

```sh
 mkdir key_backup
 cp id_rsa* key_backup
 rm id_rsa*
```

生成新的密钥

```sh
ssh-keygen -t rsa -C "xxx@xxx"              #一般不用设置密码，传输时验证很麻烦
```

然后一路Enter，生成的.ssh文件中id-rsa.pub文件存储着密钥，设置到github密钥中

```sh
ssh -T git@github.com          #检验ssh配置是否成功
cat ~/.ssh/id_rsa.pub          #查看公钥
```

问题：在生成新的密钥过程中，有时会报错：  bash:ssh-key command not found   

```text

原因：未在git安装目录操作

解决：配置环境变量usr/bin和  or  在安装目录usr/bingit下操作
```





### 首次上传

每个仓库都要新建一个本地仓库文件，在这个文件里面操作

**1.第一次初始配置**

```sh
git init                  #初始化/创建仓库
git config --list         #显示文件配置信息
git config --global user.name "Your Name"            #用户名
git config --global user.email "email@example.com"   #邮箱地址
```

**2.readme文件（非必须）**

```sh
touch REMADE.md
echo "# context" >> README.md    #readme文件写入内容 
git add README.md
```

**3.添加文件到缓冲区**

```sh
git add .                      #将当前目录和子目录所有变更文件添加到缓冲区
git add *                      #将当前目录下的变更文件添加到缓冲区
git add <filename>“            #上传指定文件到暂存区
git commit -m "first commit"   #提交变更说明
```

问题：warning: in the working copy of 'README.md', **LF** will be replaced by **CRLF** the next time Git touches it

```text
原因：windows和linux换行符转换问题（不求甚解）
解决：git config --global core.autocrlf false
```

**4.分支操作（非必须）**

```sh
git branch -M main       #强制重命名
git branch -m main       #重命名分支和相应的的引用日志
git branch               #列出本地分支
git branch -r            #列出远程分支
git branch -a            #列出全部分支
git branch -d <branch>   #删除分支
git checkout <branch>    #切换分支

```

**5.更新到远程仓库**

```sh
git remote add origin <url>   #连接远程仓库,并创建了一个origin别名
git push -u origin main       #更新到远程仓库，第一次上传-u，以后直接git push就OK了
```



## 3.拉取&提交代码

有权限用pull更新本地代码，无权限用clone单纯下载不能更新本地代码

有权限的仓库第一次下载到空仓库pull或者clone都可以

```sh
git clone <url> #无权限仓库下载代码且不适用于更新本地代码
git clone -b <checkpoint-name> <url> --deoth 1 
git pull   #有权限的仓库，必须连接远程仓库，yong'yu'geng'xin'ben'di'dai'ma
```



### 拉取代码流程

第一次本地空仓库拉取远程仓库可以用clone,不用连接即可下载

```sh
git init
git remote add origin <url>         #本地与远程仓库连接，并命名远端仓库为origin
git pull origin master              #拉取远端仓库origin到本地master分支？？？
git branch -M|m main                #建议强制重命名master为main
！！ get frtch                      #有多个远程分支，用这个命令才会显示所有远程分支

```

```text
'拉取的是默认分支，切换本地分支，用git checkout 
'切换远程分支并在本地新建一个分支对应，用 git checkout -b local-name origin/origin-name  
#远程的origin-name分支，在本地起名为local-origin分支，并切换到本地的loacl-origin分支
git checkout -b <oringin-checkout>  ???#切换远程的分支？？？？？？？？
有多个远程分支时候怎么操作，需要创建多个本地分支吗
```



### 提交代码流程

```sh
git pull origin <origin-checkout>  ？？？给i他
'在本地拉取的仓库上复制需要上传的代码'
git add .
git commit -m "first commit"
'git pull origin master        #防止版本冲突，多人合作前可忽略此条
git push -u origin <origin-checkout>      ？？？？都取名oringin是否冲突
```

问题：error: remote origin already exists.

```sh
git remote -v         #查看远程仓库是否已经关联
git remote rm origin  #删除关联的仓库
```

当我想从远程仓库里拉取一条本地不存在的分支时：

```
git checkout -b 本地分支名 origin/远程分支名
```
