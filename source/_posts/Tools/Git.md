---
title: 常用的Git命令
comments: true
date: 2019-01-26 21:20:50
categories: Tools
tags: ['Tools', 'Hexo']
---

### 新建代码库
* git init  在当前目录新建一个git代码库
* git clone [url]  下载一个项目和它的整个代码历史
* git clone git@github.com:Supremeyh/Supremeyh.github.io.git  不指定分支 
* git clone -b sea git@github.com:Supremeyh/Supremeyh.github.io.git 指定分支


### 配置信息
* git config --list  显示当前的git配置
* git config [--global] user.name "[name]"   设置提交代码时的用户名
* git config [--global] user.email "[email address]"  设置提交代码时的用户邮箱


### 查看信息
* git status 显示有变更的文件
* git log 显示当前分支的版本历史
* git log --stat 显示commit历史，以及每次commit发生变更的文件
* git log -S [keyword] 搜索提交历史，根据关键词
* git log --follow [file] 显示某个文件的版本历史，包括文件改名   eg: git log --follow package.json
* git whatchanged [file]  显示某个文件的版本历史，包括文件改名
* git log -p [file]  显示指定文件相关的每一次diff
* git log -5 --pretty --oneline  显示过去5次提交
* git shortlog -sn  显示所有提交过的用户，按提交次数排序
* git blame [file]  显示指定文件是什么人在什么时间修改过
* git diff 显示暂存区和工作区的差异
* git reflog  显示当前分支的最近几次提交
* git diff --shortstat "@{0 day ago}"  显示今天你写了多少行代码


### 分支
* git branch  列出所有本地分支
* git branch -r 列出所有远程分支
* git branch -a 列出所有本地、远程分支
* git branch sea  新建一个分支，但依然停留在当前分支
* git checkout -b  sea  新建一个分支，并切换到该分支
* git checkout  sea  换到指定分支，并更新工作区
* git checkout  -  切换到上一个分支
* git merge sea  合并指定分支到当前分支


### 删除分支
* git branch -d dev 删除本地分支
* git push origin -d dev 删除远程分支
* git push origin :dev   删除远程分支(推送一个空tag到远程tag)
* git branch | grep 'branchName' | xargs git branch -D  批量删除分支


### 标签
* git tag  列出所有tag
* git show [tag]  查看指定tag信息,  eg: git show v1.0
* git tag [tag]  新建一个tag在当前commit
* git tag -a v1.0  -m 'first version' // 创建tag
* git tag [tag] [commit]  新建一个tag在指定commit
* git tag -d [tag] 删除本地tag
* git push origin :refs/tags/[tagName]  删除远程tag
* git push origin -d tag tagName  删除远程tag
* git push [remote] [tag] 提交指定tag,  eg: git push origin v1.0 
* git push [remote] --tags  提交所有tag, eg: git push origin --tags


### 远程同步
* git remote -v  显示所有远程仓库
* git remote show [remote] 显示某个远程仓库的信息
* git remote add origin [url]   增加一个新的远程仓库，并命名
* git remote set-url origin [updated link]  设置远程仓库地址
* git remote rm origin  删除远程仓库地址
* git fetch [remote]  下载远程仓库的所有变动
* git pull [remote] [branch]  取回远程仓库的变化，并与本地分支合并
* git pull origin dev:sea  取回远程origin主机dev分支与本地sea分支合并
* git push [remote] [branch]  上传本地指定分支到远程仓库
* git push [remote] --force  强行推送当前分支到远程仓库，即使有冲突
* git push [remote] --all  推送所有分支到远程仓库


### 同步upstream 原始仓库
* git clone git@github.com:Supremeyh/blog.git   clone自己代码origin到本地
* git checkout -b dev 新建分支
* git remote -v
* git remote add upstream git@github.com:Supremeyh/blog.git   配置原始仓库
* git fetch upstream 获取原始仓库分支和对应的提交，分支dev的提交会保存到本地分支，upstream/dev
* git rebase upstream/dev  要经常与主干保持同步
* git rebase -i upstream/dev  合并commit
* git checkout dev  切换到fork仓库本地的dev分支
* git merge upstream/dev  把原始upstream/dev的改变合并到本地的dev分支
* git push  推送自己的本地仓库到自己的origin远程仓库
* 发出Pull Request


### 放弃本地修改，代码强制拉取更新 
* git fetch --all 
* git reset --hard origin/master 
* git pull //可以省略


### 撤销 commit
* git log  找到你想撤销的commit_id
* git reset HEAD^ 不删除工作空间改动代码，撤销commit，并且撤销git add .
* git reset --soft HEAD^  不删除工作空间改动代码，撤销commit,不撤销git add，尝试回退一个版本
* git reset --hard HEAD~2  尝试回退两个版本
* git reset --hard [commit_id] 完成撤销,同时将代码恢复到前一commit_id 对应的版本
* git reset [commit_id] 完成Commit命令的撤销，但是不对代码修改进行撤销
* git commit --amend   若commit注释写错了，只是想改一下注释，进入默认vim编辑器，修改注释完毕后保存即可

注:  git reset HEAD^  
--mixed，默认参数，等同于 git reset --mixed HEAD^ 
--soft， 不删除工作空间改动代码，撤销commit，不撤销git add . 
--hard， 删除工作空间改动代码，撤销commit，撤销git add . 。完成这个操作后，就恢复到了上一次的commit状态

HEAD^指上一个版本，也即HEAD~1


### 撤销 push到远端后的操作
* git log remotes/origin/分支名  查看版本号
* git reset --hard <需要回退到的版本号（只需输入前几位）>   先在本地回退到需要的版本, 本地不需要回退可略过
* git push origin branchName --force   提交到远端


### 暂存
* git stash 'some message' 将撤销的代码暂存起来。 暂时将未提交的变化移除，稍后再移入
* git stash pop  重新应用缓存
* git stash list


### FQA
#### .gitignore不生效
* git rm -r --cached .  //清空缓存

####  Permission denied (publickey).
* cd ~/.ssh  ls  来查看是否有文件id_rsa以及文件id_rsa.pub
* ssh-keygen -t rsa -C "supremeyh@126.com"   生成ssh key
* ssh -v git@github.com
* ssh-agent -s
* ssh-add ~/.ssh/id_rsa  
* cat id_rsa.pub   拷贝内容到github，在settings下，SSH and GPG keys下new SSH key的key 中保存
* ssh -T git@github.com  验证key


#### 将本地的分支与远程仓库的分支进行关联
 The branch 'master' has no upstream branch.Would you like to publish this branch ？ 
 在vscode 通过 push 快捷操作时，出现弹框提示。问题的原因是没有将本地的分支与远程仓库的分支进行关联。 
出现这种情况主要是由于远程仓库太多，且分支较多。在默认情况下，git push时一般会上传到origin下的master分支上，然而当repository和branch过多，而又没有设置关联时，git就会产生疑问，因为它无法判断你的push目标。 
* git branch -a  查看所有分支

方法一
* git push --set-upstream origin master   保证你的远程分支存在，如果不存在，也就无法进行关联
方法二（推荐）
* git push -u origin master  即使远程没有你要关联的分支，它也会自动创建一个出来，以实现关联


### 插件
* Octotree 侧栏目录树形