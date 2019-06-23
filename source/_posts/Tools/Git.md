---
title: 常用的Git命令
comments: true
date: 2019-01-26 21:20:50
categories: Tools
tags: ['Tools', 'Hexo']
---

### 查看日志
* git log 
* git reflog 

### 拷贝代码
* git clone git@github.com:Supremeyh/Supremeyh.github.io.git  不指定分支 
* git clone -b sea git@github.com:Supremeyh/Supremeyh.github.io.git 指定分支

### 取回远程origin主机dev分支与本地sea分支合并
* git pull origin dev:sea

### 放弃本地修改，代码强制拉取更新 
* git fetch --all 
* git reset --hard origin/master 
* git pull //可以省略

### 设置远程url
* git remote -v //查看url
* git remote set-url origin [updated link]

### .gitignore不生效
git rm -r --cached .  //清空缓存

### tag
* git tag,  或 git show v1.0   // 显示tag信息
* git tag -a v1.0  -m 'first version' // 创建tag
* git push origin v1.0 , 或者 git push origin --tags // 共享tag
* git tag -d v1.0 // 删除tag


###  Permission denied (publickey).
* cd ~/.ssh  ls  来查看是否有文件id_rsa以及文件id_rsa.pub
* ssh-keygen -t rsa -C "supremeyh@126.com"   生成ssh key
* ssh -v git@github.com
* ssh-agent -s
* ssh-add ~/.ssh/id_rsa  
* cat id_rsa.pub   拷贝内容到github，在settings下，SSH and GPG keys下new SSH key的key 中保存
* ssh -T git@github.com  验证key

### 修改远程仓库地址
* git remote -v  查看
* git remote rm origin  删除
* git remote add origin ssh://git@repository.git   新增


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


### 将本地的分支与远程仓库的分支进行关联
 The branch 'master' has no upstream branch.Would you like to publish this branch ？ 
 在vscode 通过 push 快捷操作时，出现弹框提示。问题的原因是没有将本地的分支与远程仓库的分支进行关联。 
出现这种情况主要是由于远程仓库太多，且分支较多。在默认情况下，git push时一般会上传到origin下的master分支上，然而当repository和branch过多，而又没有设置关联时，git就会产生疑问，因为它无法判断你的push目标。 
* git branch -a  查看所有分支

方法一
* git push --set-upstream origin master   保证你的远程分支存在，如果不存在，也就无法进行关联
方法二（推荐）
* git push -u origin master  即使远程没有你要关联的分支，它也会自动创建一个出来，以实现关联


### 批量删除分支
git branch | grep 'branchName' | xargs git branch -D

### 插件
* Octotree 侧栏目录树形