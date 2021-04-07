---
title: git | 常用命令
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-27 15:27:31
password: false
summary: false
tags:
    - 博客
    - git
    - 命令
categories:
    - git
    - 随笔
---

#### 1、git初始化仓库
```
   git init  创建一个.git目录，跟踪管理版本
```
#### 2、 git 添加文件
```
 git add xxx.xxx 添加文件到暂缓存区里
 git add .  添加目录下所有文件
```
#### 3、git提交
```
  git commit -m  "提交文字说明"
```
#### 4、查看提交状态
```
 git status
```

#### 5、查看文件修改内容
```
git diff xxx.xxx (具体文件名称) 
```

#### 6、查看历史记录

```
get log 

git log -n(n表示前N条)
```

> - 如果不带任何参数，它会列出所有历史记录，最近的排在最上方，显示提交对象的哈希值，作者、提交日期、和提交说明。如果记录过多，则按Page Up、Page Down、↓、↑来控制显示；按q退出历史记录列表。
> - 如果不想向上面那样全部显示，可以选择显示前N条。

```
如两天前的提交历史：git log --since=2.days

如指定作者为"BeginMan"的所有提交:$ git log --author=BeginMan

如指定关键字为“init”的所有提交：$ git log --grep=init

如指定提交者为"Jack"的所有提交：$ git log --committer=Jack

注意作者与提交者的关系：作者是程序的修改者，提交者是代码提交人。
```

如指定2天前，作者为“BeginMan”的提交含有关键字'init'的前2条记录：

> git log --since=2.days --author=BeginMan --grep=init -2


查看某次commit做了哪些修改


```
  git log                       #查看commit的历史
  git show <commit-hash-id>     #查看某次commit的修改内容
```

#### 7、获取版本号

```
  git reflog
```
#### 8、版本回退
```
  git reset --hard HEAD^ 回退上一个版本
  git reset --hard HEAD^^ 回退上上个版本

  git reset --hard HEAD~n 回退前n个版本
  git reset --hard xxxxxx # 回退到某个版本回退点之前的所有信息。 (输入版本号)
  git reset --hard origin/master    # 将本地的状态回退到和远程的一样 
  git  reset  xxxxxx  # 回退到指定版本(输入版本号)
```
#### 9、关联远程仓库
```
  git remote add origin xxx (xxx为github/码云/其他仓库的项目地址)
```
#### 10、从仓库更新代码
```
  git pull
  
  git pull origin
```
将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并。
> git pull origin master:brantest
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
> git pull origin master
#### 11、推送到远程库
```
  git push -u origin master
  
  git push
  
  git push --all origin
```
> - (1)、 git push -u origin master 如果当前分支与多个主机存在追踪关系，则可以使用 -u 参数指定一个默认主机，这样后面就可以不加任何参数使用git push，不带任何参数的git push，默认只推送当前分支，这叫做simple方式，还有一种matching方式，会推送所有有对应的远程分支的本地分支， Git 2.0之前默认使用matching，现在改为simple方式,如果想更改设置，可以使用git config命令。git config --global push.default matching OR git config --global push.default simple；可以使用git config -l 查看配置
> - (2)、 git push --all origin 当遇到这种情况就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要 -all 选项
> - (3)、 git push --force origin git push的时候需要本地先git pull更新到跟服务器版本一致，如果本地版本库比远程服务器上的低，那么一般会提示你git pull更新，如果一定要提交，那么可以使用这个命令。
> - (4)、 git push origin --tags //git push 的时候不会推送分支，如果一定要推送标签的话那么可以使用这个命令

#### 12、克隆远程仓库
```
  git clone 链接
```
#### 13、操作远程仓库
```
　1.git remote 不带参数，列出已经存在的远程分支

　2.git remote -v | --verbose 列出详细信息，在每一个名字后面列出其远程url，此时， -v 选项(译注:此为 –verbose 的简写,取首字母),显示对应的克隆地址。

　3.git remote add url   添加一个远程仓库
```
#### 14、分支
```
  git checkout -b dev 创建分支 
  
  git branch 查看当前分支
  
  git checkout master 切换到分支master
  
  git merge dev 把master分支合并到dev
  
  git branch xx 创建分支
  
  git branch -d xx 删除分支xx
```
以上只是罗列比较常用的命令，如果需要查看更详细的内容，大家可以去[菜鸟教程](  https://www.runoob.com/git/git-create-repository.html)的git教程学习，里面详细列举了git的所有使用方式。
