# Git教程笔记

- 学习参考网站：https://www.liaoxuefeng.com/wiki/896043488029600
- 关键命令的记录和注释
- 记录git基本原理和图表注解

## [创建版本库](https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304)

    git init    # 创建仓库  
    git add readmetxt   # 添加readme.txt到仓库  
    git commit -m "wrote readme file"   # 文件提交到仓库，"-m"后面输入的是本次提交的说明  

## 时光机穿梭

### [版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)

    git log # 查看提交历史，commit id（版本号）
    git log --pretty=oneline    # 单行显示提交历史信息  
    git reset --hard HEAD^      # 回退到上一个版本（上一个版本：HEAD^，上上一个版本：HEAD^^，往上100个版本：HEAD~100） 
    git reset --hard 1094a      # 向后恢复到指定版本  
    git reflog                  # 查看回溯命令历史（移动Head指针） 

### [工作区和暂存区](https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576#0)

工作区（含有.git的当前目录）->stage（暂存区）->master（分支）

![resp_1](images/resp_1.jpg)

    git status  # 查看状态
    git add .   # 添加所有未更新文件
    git commit -m "understand how stage works"
    git status  # 工作区是否“干净”

### [管理修改](https://www.liaoxuefeng.com/wiki/896043488029600/897884457270432)

    cat readme.txt  # 捉取readme.txt内容
    git diff HEAD -- readme.txt # 查看工作区和版本库里面最新版本的区别

注意事项：第一次修改 -> git add -> 第二次修改 -> git add -> git commit，防止第二次修改未提交

### [撤销修改](https://www.liaoxuefeng.com/wiki/896043488029600/897889638509536)

    # 场景1：仍在工作区
    git checkout -- file    # 撤销工作区的修改
    # 场景2：已提交到暂存区
    git add file
    git reset HEAD <file>   # 撤销暂存区的修改（unstage）
    git checkout -- file    # 撤销工作区修改
    # 场景3：提交到版本库但未推送到远程库
    # 参考“版本回退”小节

注意事项：
1. 命令git checkout -- readme.txt会将readme.txt文件在工作区的修改全部撤销，此处分两种情况：(a) readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；(b) readme.txt已经添加到暂存区后，又作了修改，若撤销修改就回到添加到暂存区后的状态，即让这个文件回到最近一次git commit或git add时的状态
2. git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令


## [远程仓库](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

- 本地Git仓库<->GitHub<->第三方
- 本地Git仓库和GitHub仓库由SSH加密传输

        ssh-keygen -t rsa -C "youremail@example.com"  
        ...一直回车

-  SSH Key密钥对：id_rsa（私钥，不可泄露）和id_rsa.pub（公钥）
-  GitHub->“Account settings”->“SSH Keys”->“Add SSH Key”->填上任意Title->Key文本框里粘贴id_rsa.pub文件的内容
-  原文更正：[GitHub 重磅更新：无限私有仓库免费使用](https://www.infoq.cn/article/aKm94Aw1RmDL_9Gysm8D)


### [添加远程库](https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440#0)

    git remote add origin https://github.com/fengyihuai/learngit.git    # 关联本地库到远程库，推荐https链接方式
    git push -u origin master    # 把本地库的所有内容推送到远程库
    git push origin master       # 本地master分支的最新修改推送至GitHub

注意事项：
1. 第一次推送master分支时，若加上了-u参数，Git在将本地的master分支内容推送的远程新的master分支同时，额外关联本地的master分支和远程的master，在后续的推送或者拉取中就可以简化命令，master分支为默认推送目标
2. SSH警告 当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
  
>The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.  
>RSA key fingerprint is xx.xx.xx.xx.xx.  
>Are you sure you want to continue connecting (yes/no)?  

详细处理请访问本标题网页


