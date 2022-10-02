

# Git 笔记

### 一. git 基础知识点

1. git控制的区域

   git控制的区域可以分为工作区(working tree)、缓冲区(stage)、版本区(commit [repository)，工作区和缓冲区是所有分支共用的，版本区每个分支都有各自的。对文件的修改会即时的保存在工作区，只有把文件添加到缓冲区才能提交到版本区。

2. git分支管理

   - 创建分支

     创建git仓库的时候，会默认创建主分支main，在开发的过程中需要从主分支开辟出一条新的分支dev，而每个程序员又要从dev分支上开辟出一条自己的任务分支work。

   - 合并分支

     开辟的分支都要合并到提交到主分支main才算完成一个版本。

   - 解决分支冲突

     在项目开发中难免会出现不同程序员对同一文件同一段代码的修改，此时合并就会产生冲突，git就会自动标注出冲突的位置，需要程序员之间协调进行手动修改。

### 二. 常用的git操作

1. 初始化git仓库
在项目的需要的管理的文件夹中执行命令，会在该文件夹内生成隐藏的 .git 文件夹 `git init`

2. 添加文件到缓冲区
首先是确认当前所在分支，分支名称前有 * 符号，表示自己当前所在分支的位置 `git branch`，然后查看工作区的状态 `git status`，最后确认状态后，添加到缓冲区 `git add <filie-name>`

3. 提交文件到版本区，生产一个新的版本结点 `git commit -m “提交携带的信息”`

4. 恢复修改
   恢复修改分为三种情况：
   \- 第一种：文件未添加到缓存区
   先 `git diff`，查看工作区和缓存区文件到差异，可以手动修改，也可以用命令 `git restore <filename>` 回退修改，与缓存区的同一份文件保持一致。

   \- 第二种：文件已经提交到缓存区，但还提交到版本区(commit repository) 
   先 `git diff --cached`，查看缓存区的和版本区中最近一次commit的区别，如果不想把缓存区的修改提交到版本区，同时想将缓冲区的文件回退和版本区保持一致，使用命令 `git restore --staged <file-name>`

   \- 第三种：文件已经提交到版本区，此时只能回退到想要的版本。
   先`git log`查看一下提交操作，根据提交信息查看想要回退到的版本的commit-id ，然后用`git reset --hard <commit-id>`可以回退版本。

5. 删除文件
   先 `rm <file-name>` 删除工作区的文件，然后 `git rm <file-name>`添加到工作区，最后提交到版本区

6. 分支管理
   新建分支
   `git branch <branch-name>` 或者`git switch -c <branch-name>`

   分支合并
   先 `git switch <branch-fathername>` 切到想要合并到分支的父分支上，然后 `git merge <branch-sonname>` 合并子分支， `git merge` 会在父分支上完成一次提交，生产一个commit。然后删除分支，`git branch -d <branch-name>`

   暂存分支状态
   当某一分支上的工作没有完成还不能提交到分支的版本库中时，可以用 `git stash` 暂存该分支当时的状态，从而转到其他的分支区处理新的工作。回到该分支继续工作，`git stash pop` 弹出之前保存的stash，即可接着处理。
   也可以多次 `git stash`，通过 `git stash list` 查看所有的stash，找到该分支上的stash@{0}，再 `git stash apply stash@{0}` 回到当时的状态，最后删除stash `git stash drop stash@{0}` 

7. 多人协作
   查看远程库信息，使用`git remote -v`。
   本地新建的分支如果不推送到远程，对其他人就是不可见的。从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交。
   在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
   建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

8. 还没写完，先暂时保存一下

   