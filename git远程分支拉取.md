## git 远程分支拉取


### “origin” 并无特殊含义
远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。 同时 “master” 是当你运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用，“origin” 是当你运行 git clone 时默认的远程仓库名字。 如果你运行 git clone -o void，那么你默认的远程分支名字将会是void/master。

如果在本地的 master 分支做了一些工作，然而在同一时间，其他人推送提交到远程仓库，并更新了远程仓库的master分支，那么远程仓库的提交历史将向不同的方向前进。 但是，只要本地不与 origin 服务器连接，本地的 origin/master 指针就不会移动。

### 如果要拉取master分支上的更新
如果要拉取其他人推送到master分支上的更新，同步自己的工作，就需要git fetch 命令来拉取，运行 git fetch origin 命令。 这个命令查找 “origin” 是哪一个服务器，(本地仓库记录了当前目录的绑定的远程代码仓库服务器地址)，如github上的远程仓库，从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针指向新的、更新后的位置。但是只是用git fetch 是不会合并远程仓库的更新到本地代码仓库。可以使用命令：git log -p master.. origin/master来查看更改，然后通过 git merge origin/master 合并更新。
git fetch origin
git log -p master
git merge origin/master

同：
git fetch origin master:temp //git branch temp
git diff temp
git merge temp // temp 合并到 master
