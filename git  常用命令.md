# git 常用命令

---

1.[廖雪峰官方网站](https://www.liaoxuefeng.com/wiki/896043488029600)

---

## 1.新建代码库

>//在当前目录新建一个Git代码库
>git init [project-name]
>
>//克隆一个项目和他的整个代码历史
>git clone [url]

## 2.配置

>//显示当前git配置
>git config -list
>//编辑git配置文件 --global 全局 默认当前文件
>git config -e [--global]
>//设置提交代码的用户信息
>git config [--global] user.name "[name]"
>git config [--global] user.email "[email]"

## 3.增加/删除文件

> //添加指定的文件到暂存区 
>git add [file] [file2] ...
>// 添加指定文件夹到暂存区，包括子目录
>git add [dir]
>// 添加当前所有文件到暂存区
>git add .
>//添加每个变化前，都会要求确认，对于同一个文件的多处变化，可以实现分次提交
>git add -p
>//删除工作区文件，并且将这次删除放到暂存区
>git rm [file1] [file2] ...
>// 停止跟踪指定文件，但该文件会保留在工作区
>git rm --cached [file]
>//更该文件名，并且将这个改名放到暂存区
>git mv [file-original] [file-renamed]

## 4.代码提交

>//提交暂存区代码到仓库区
>git commit -m [message]
>//提交暂存区的指定文件到仓库区
>git commit [file] [file2] ... -m [message]
>//提交工作区自上次commit之后的变化，直接到仓库区
>git commit -a
>//提交时显示所有diff 信息
>git commit -V
>//使用一次新的commit，替代上一次的提交
>//如果代码没有任何变化，则用来改写上一次commit的提交信息
>git commit --amend -m [message]
>//重新上一次的提交，并包含指定文件的新变化
>git commit --amend [file1] [file2] ...

## 5.分支

>//列表本地所有分支
>git branch
>//列出所有远程分支
>git branch -r
>//列出所有本地和远程分支
>git branch -a
>//新建一个分支，指向指定commit
>git branch [branch] [commit]
>//新建一个分支，与指定分支的远程分支建立追踪关系
>git branch --track [branch] [remote-branch]
>//切换到指定分支，并更新工作区
>git checkout [branch-name]
>//切换到上一个分支
>git checkout -
>//建立追踪关系，在现有分支与指定的远程分支之间
>git branch --set-upstream [branch] [remote-branch]
>//合并指定分支到当前分支
>git merge [branch]
>//选择一个commit，合并进当前分支
>git cherry-pick [commit]
>//删除分支
>git branch -d [branch-name]
>//删除远程分支
>git push origin --delete [branch-name]
>git branch -dr [remote/branch]

## 6.标签

>//列出所有tag
>git tag
>//新建一个tag在当前commit
>git tag [tag]
>//新建一个tag在指定commit
>git tag [tag] [commit]
>//删除本地tag
>git tag -d [tag]
>//删除远程tag
>git push origin :ref/tags/[tagname]
>//查看tag信息
>git show [tag]
>//提交tag信息
>git push [remote] [tag]
>//提交所有tag
>git push [remote] --tags
>//新建一个分支，指向某个tag
>git checkout -b [branch] [tag]

## 7.查看信息

>//显示所有变更的文件
>git status
>//显示当前分支的版本历史
>git log
>//显示commit提交历史，以及每次commit发生变更的文件
>git log --stat
>//搜索提交历史，根据关键词
>git log -S [kwyword]
>//显示某个commit之后的所有变动，每个commit占据一行
>git log [tag] HEAD --pretty=format:%s
>//显示暂存区和工作区的差异
>git diff
>//显示暂存区和上一个commit的差异
>git diff --cached [file]
>//显示工作区与当前分支最新commit之前的差异
>git diff HEAD
>//显示两次提交之间的差异
>git diff [first-branch] [second-branch]
>//显示某次提交的元数据和内容变化
>git show [commit]
>//显示某次提交发生变化的文件
>git show --name-only [commit]
>//显示某次提交时，某个文件的内容
>git show [commit]:[filename]
>//显示当前分支的最近几次提交
>git relog

## 8.远程同步

>//下载远程仓库的所有变动
>git fetch [remote]
>//显示所有远程仓库
>git remote -v
>//显示某个远程仓库的信息
>git remote add [shortname] [url]
>//取回远程仓库的变化，并与本地分支合并
>git pull [remote] [branch]
>//上传本地指定分支到远程仓库
>git push [remote] [branch]
>//强行推送当前分支到远程仓库，即使有冲突
>git push [remote] --force
>//推送所有分支到远程仓库
>git push [remote] --all

## 9.撤销

>//恢复暂存区的指定文件到工作区
>git checkout [file]
>//恢复某个commit的指定文件到暂存区和工作区
>git checkout [commit] [file]
>//恢复暂存区的所有文件到工作区
>git checkout .
>//重置暂存区的所有文件，与上一次commit保持一致，但工作区不变
>git reset [file]
>//重置暂存区与工作区，与上一次commit保持一致
>git reset --hard
>//重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
>git reset [commit]
>//重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与制动commit一致
>git reset --hard [commit]
>//重置当前HEAD为指定commit，但保持暂存区和工作区不变
>git reset --keep [commit]
>//新建一个commit，用来撤销指定commit，
>//后者的所有变化都将被前者抵消，并且应用到当前分支
>git revert [commit]
>//暂时将未提交的变化移除，稍后再移入
>git stash
>git stash pop

## 10.rebase

>//合并多个commit为一个完整commit
>git rebase -i  [startpoint]  [endpoint]
>git rebase -i HEAD~3
>
>
>
>