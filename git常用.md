## git 常用


- 官网: [http://git-scm.com/downloads](http://git-scm.com/downloads)
- 官方推荐的: [Pro Git 第二版中文版](https://bingohuang.gitbooks.io/progit2/content/)
- github最近start陡增的项目: [Github-Ranking](https://github.com/EvanLi/Github-Ranking)
- https://gitmoji.dev/



### 常用设置
- 添加一个配置
    - `git config --global core.editor "vim"`
- 列出所有配置
    - `git config -l`

```text
http.sslverify=true
http.sslcapath=/home/ghostwwl/miniconda3/ssl/cacert.pem
http.sslcainfo=/home/ghostwwl/miniconda3/ssl/cacert.pem
user.email=wule@jd.com
user.name=wule
core.autocrlf=input
core.editor=vim
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true


remote.origin.url=git@git.jd.com:intelligent_monitor_group/app_risk_assessment.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.test.remote=origin
branch.test.merge=refs/heads/test
branch.dev.remote=origin
branch.dev.merge=refs/heads/dev
branch.pre.remote=origin
branch.pre.merge=refs/heads/pre
branch.master.remote=origin
branch.master.merge=refs/heads/master

```

- 对已提交的注释修改
    - `git commit --amend`

- 修改本地的全局配置 `vim ~/.gitconfig`
```bash

[user]
	email = wule@jd.com
	name = wule
[core]
	autocrlf = input
	editor = vim
[rebase]
	autostash = true
[alias]
	co = checkout
	ci = commit
	br = branch
	mg = merge
	st = status
	cp = cherry-pick
	cpa = cherry-pick --abort
	# ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad %C(red)%d %C(green)[%cn] %C(reset)%s\" --decorate --date=short
	ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad %C(red)%d %C(green)[%cn] %C(reset)%s\" --decorate --date=format:\"%Y/%m/%d %H:%M\"
	hist = log --pretty=format:\"%C(yellow)%h %C(blue)%ad %C(red)%d %C(green)[%an] %C(reset)%s\" --topo-order --graph --date=format:\"%Y/%m/%d %H:%M:%S\"
	lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
[color]
	ui = auto
  	diff = auto
  	branch = auto
  	status = auto
[color "branch"]
	current = yellow reverse
  	local = yellow
  	remote = green
[color "diff"]
	meta = yellow
  	frag = magenta
  	old = red
  	new = green
[color "status"]
	added = yellow
  	changed = green
  	untracked = cyan
[log]
	date = iso-strict-local


```



### 查看某段代码是谁写的
- `git blame <file-name>`
- 查看两个星期内的改动
    - `git whatchanged --since='2 weeks ago'`


### 分支导入导出
- 把某个分支导出为一个文件
    - `git bundle create <file> <branch-name>`
- 从包中导入分支(新建一个分支，分支内容就是上面的 git bundle create 导出的内容)
    - `git clone repo.bundle <repo-dir> -b <branch-name>`



### 获取某个分支最后提交
- `git rev-parse branch_name^{commit}`
- 获取`master`分支最后提交 
    - `git rev-parse master^{commit}` --> 得到一个 commit hash 例如 `e85f058037adcf2c086ba9c9629177e3d99a7fd2`
    - 拉取指定的提交 `git checkout -f e85f058037adcf2c086ba9c9629177e3d99a7fd2`


### 从某次提交拉出新版本

- `git log` 出想拉出分支的最后提交的 commitid 并复制
- `git checkout commitid -b new_branch_name` commitid 为第一步查到的 commitid,  new_branch_name 本地新拉出分支的名称



### 展示文件相关
- 展示任意分支某一文件的内容
    - `git show <branch-name>:<file-name>`

- 展示所有 tracked 的文件
    - `git ls-files -t`

- 展示所有 untracked 的文件
    - `git ls-files --others`

- 展示所有忽略的文件
    - `git ls-files --others -i --exclude-standard`

- 强制删除 untracked 的文件(删除后无法找回)
    - `git clean <fille-name> -f`

- 强制删除 untracked 的目录(这个也可以用来删除untracked的文件)
    - `git clean <directory-name> -df`

- 清除 gitignore 文件中记录的文件(这个可以用于安规中清理文件了)
    - `git clean -X -f`



### git diff 相关

- 查看已经 add 没有 commit 的改动
    - `git diff --cached`
    
- 查看当前没有 add 和 commit 的改动：
    - `git diff HEAD`
    - `git status`

- 比较当前版本和上一个版本的区别
    - `git diff HEAD HEAD^`
    - `git diff HEAD HEAD~1`
    
- 比较当前版本与上两个版本的区别
    - `git diff HEAD HEAD~2`
    
- 比较本地 master 和 远程 master 的区别
    - `git diff master origin/master`
    -  只看文件改动 `git diff --stat master origin/master`
    -  统计本地两个分支 `git diff --stat master dev`
    
- 利用版本号比对
    - 查看版本号 `git reflog`
        ```text
        a0fe548 (HEAD -> master, origin/master) HEAD@{0}: commit: 修复手抖...
        04a6abe HEAD@{1}: commit: 下游接口异常情况...98f4879 HEAD@{4}: commit: 整体超时调用修改
        ee2c09e HEAD@{2}: commit: 618机器资源相关
        3be8731 HEAD@{3}: commit: 品牌接口相关参数
        27237e8 HEAD@{4}: commit: 相关代码清理
        9acbd15 HEAD@{5}: commit: 依赖包相关修改
        1b90a28 HEAD@{6}: commit: 启动进程数目
        8f9844d HEAD@{7}: commit: 调试相关修改
        ```
    - `git diff a0fe548 04a6abe`
    
- 比较两个版本号下面的 某个 文件/文件夹 的差异
    - `git diff a0fe548 04a6abe conf`


- 比较两个分支下的 某个 文件的差异
    - `git diff branch1 branch2 -- path/file.txt`


### 提交后发现某些漏改
- `git commit --amend`


### 增补提交，不会产生新的历史提交记录
- `git commit -C head -a --amend`




### 展示本地分支关联远程仓库的情况
- `git branch -vv`



### git merge
- `git merge --abort` 两个分支merge时出现冲突，取消merge
- `git merge <branch>` 合并指定分支到当前分支 
- `git merge --no-ff <branch>` 合并分支的时候禁用 Fast forward 模式,因为这个模式会丢失分支历史信息


```bash

# 如: 健康度发布 dev --> merge 到 pre --> merge 到 master 线上分组用 master 分支构建镜像

# 把 dev 分支 merge 到 pre 分支
git checkout pre
git merge --no-ff dev

# 把 pre 分支 merge 到 master 分支
git checkout master
git merge --no-ff pre

```


### git pull 详细
- git pull --progress -v



### git log 查看提交历史

- 查看某个文件的提交历史
    - `git log -p <file>`
    - `git blame <file>` 以列表方式查看指定文件历史
    - `git blame -L 12,22 <file>` 限制输出范围在第12至22行

- 显示每次提交的内容差异 
    - `git log -p -2`
        - 用来显示每次提交的内容差异
        - 仅显示最近两次提交
- 显示每次提交的简略统计信息
    - `git log --stat`
- 自定义非默认显示格式
    - `git log --pretty=oneline`
        - oneline 将每个提交放在一行显示
        - short 显示作者
        - full  显示作者和提交者
        - fuller 显示作者和提交者 添加时间和提交时间
        - format 自定义格式
            - `git log --pretty=format:"%h - %an, %ar : %s" --graph` 
            - `git log --pretty=format:"%h - %an, %ar : %s" --graph` 
            - `git log --pretty=format:"%h %s" --graph`
- branch之间的关系图
    - `--decorate` 标记会让git log显示每个commit的引用(如:分支、tag等)
    - `--simplify-by-decoration` 只显示被branch或tag引用的commit
    - `git log --graph --decorate --simplify-by-decoration --all`
    - `git log --graph --decorate --oneline --simplify-by-decoration --all`
    - `git log --graph --decorate --simplify-by-decoration --all`
    
    
### 撤销操作

- 回滚到远程仓库的状态
    - `git fetch --all && git reset --hard origin/master`

- 反转最近一次提交但不提交
    - `git revert --no-commit head`

- 撤销工作目录中所有未提交
    - `git reset --hard HEAD`

- 返回到某个节点，不保留修改
    - `git reset --hard HASH`

- 返回到某个节点，保留修改
    - `git reset --soft HASH`

- 撤消暂存的文件
    - `git add *`
    - `git status`
    - `git reset HEAD xxxxfile`
    
- 撤消对文件的修改
    - `git status`
    - `git checkout -- <file>`

- 撤销对文件的修改
    - `git checkout HEAD <file>`


- 撤销本地所有修改, 没有的提交的，都返回到原来的状态
    - `- git checkout .  `

- 撤销指定的提交(以新增一个commit的方式还原某一个commit的修改)
    - `git revert <commit>`


- 回到某个commit的状态， 并删除后面的commit
    > 和revert的区别: reset 命令会抹去某个commit id之后的所有commit

```bash
git reset <commit-id> #默认就是-mixed参数。

git reset –mixed HEAD^ #回退至上个版本， 它将重置HEAD到另外一个commit,并且重置暂存区以便和HEAD相匹配，但是也到此为止。 工作区
不会被更改。

git reset –soft HEAD~3 #回退至三个版本之前， 只回退了commit的信息， 暂存区和工作区与回退之前保持一致。如果还要提交， 直接commit
即可

git reset –hard <commit-id> #彻底回退到指定commit-id的状态， 暂存区和工作区也会变为指定commit-id版本的内容

```


### 重命名和删除
- 重命名 `git mv <old> <new>`
- 删除文件 `git rm <file>`
- 停止跟踪文件，但不删除 `git rm --cached <file>`
    


### 远程仓库操作相关 

- 添加远程仓库
    - git remote add rname http://xxx.xxx.xxx/xxxa.git
        - `rname` 为给远程仓库取的名字 
    - git remote -v
    - git fetch rname

- 修改原厂仓库的url
    - `git remote set-url origin <remote-url>`


    
- 远程仓库重命名
    - git remote rename origin og
    
- 远程仓库删除
    - git remote rm og
    
- 从远程仓库中抓取与拉取
    - git fetch origin
        - 果使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写
        - 抓取克隆(或上一次抓取)后新推送的所有工作 它并不会自动合并或修改当前的工作

- 推送到远程仓库
    - git push origin master

- 查看远程仓库
    - git remote show origin    
    


### 对库进行优化
> git maintenance run


    
### 解决多人协作开发的冲突问题 常用

- 放弃本地修改
    - `git checkout head .`
    
- 解决冲突后提交本地修改
    - `git stash` 将本地修改放入工作现场(成功后本地工作区间的代码跟本地仓库代码会同步,回退到上次没有修改的版本)
        - `git stash save "存储说明"`
        - 直接 "git stash" 则将上次 commit 注释作为说明 
    - `git pull` 从远程仓库获取最新代码
    - `git stash pop` 取出本地修改的代码，手动解决冲突后执行git add -A,git commit进行代码提交


### git 强制覆盖本地 (回到远程仓库的状态)

- git fetch --all  
- git reset --hard origin/master 
- git pull

- 第二种方法
    - git reset --hard HEAD
    - git pull


### Git隐藏(Stash)操作

- git status
```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   main.py

no changes added to commit (use "git add" and/or "git commit -a")
```
- git stash
    - 切换分支以进行其它分支修改，但不想提交一直在做的工作; 
    - 那么可以把当前工作的改变隐藏起来
- git stash save
    - 作用等同于git stash 不过可以添加一些注释容易查找  
    - 例如 git stash save "干了一半要搞别的"
- git status
    - 工作目录是干净的，所有更改都保存在堆栈中
    - 可以切换到其它分支去干活了
- git stash list
    - 干完其它分支的活，来查看已存在更改的列表
- git stash pop
    - 重新开始上次没有写完成的代码，从堆栈中删除更改并将其放置在当前工作目录中
- git stash drop stash@{$num} 
    - 丢弃stash@{$num}存储，从列表中删除这个存储
- git stash clear 
    - 删除所有缓存中的数据
- git stash branch
    - 从最新的stash创建分支
- git stash show -p 
    - 显示第一个存储的改动，如果想显示其他存存储，命令：git stash show  stash@{$num} -p ，
    - 显示第二个：git stash show  stash@{1} -p

- git stash apply stash@{id}
    - 恢复指定id的stash内容，同时不会删除恢复的缓存条目。
- git stash pop
    - 恢复最近的缓存到当前文件中，同时删除恢复的缓存条目


### git cherry-pick 对已经存在的commit 进行 再次提交 

git cherry-pick可以选择某一个分支中的一个或几个commit(s)来进行操作。例如，假设我们有个稳定版本的分支，叫v2.0，
另外还有个开发版本的分支v3.0，我们不能直接把两个分支合并，这样会导致稳定版本混乱，但是又想增加一个v3.0中的功能到v2.0中，
这里就可以使用cherry-pick了

- git cherry-pick <commit id>
    - 注意：当执行完 cherry-pick 以后，将会 生成一个新的提交；
    - 这个新的提交的哈希值和原来的不同，但标识名 一样；

- 对 branch 
$ git checkout old_cc
$ git cherry-pick 38361a68    # 这个 38361a68 号码，位于：

 
$ git log
commit 38361a68138140827b31b72f8bbfd88b3705d77a
Author: Siwei Shen <siwei.shen@focusbeijing.com>
Date:   Sat Dec 10 00:09:44 2011 +0800

1. 如果顺利，就会正常提交。结果：
```
Finished one cherry-pick.
# On branch old_cc
# Your branch is ahead of 'origin/old_cc' by 3 commits.
```

2. 如果在cherry-pick 的过程中出现了冲突
```
Automatic cherry-pick failed.  After resolving the conflicts,
mark the corrected paths with 'git add <paths>' or 'git rm <paths>'
and commit the result with:

        git commit -c 15a2b6c61927e5aed6718de89ad9dafba939a90b
```

就跟普通的冲突一样，手工解决：
- 看哪些文件出现冲突
    - git status
- 手动修改冲突
    - vim dira/dirb/xxx.file
- git add dira/dirb/xxx.file
- git commit -c <新的commit号码>



### clone远程所有分支 

默认只能clone远程库的master分支，
无法clone所有分支的解决办法如下

> 如果项目太大 clone 失败, 可以: git clone --depth 1 xxx.git 进行浅 clone

```
找一个干净目录，假设是 git_work 
cd git_work
git clone http://myrepo.xxx.com/project/.git ,这样在git_work目录下得到一个project子目录
cd project
git branch -a，列出所有分支名称如下：
remotes/origin/dev
remotes/origin/release
```


#### 基于远程分支新建本地分支
- `git checkout -b dev origin/dev`，作用是 checkout 远程的 dev 分支，在本地起名为 dev 分支，并切换到本地的 dev 分支
- `git checkout -b release origin/release`，作用参见上一步解释
- `git checkout dev`，切换回 dev 分支，并开始开发。 新版可以为  git switch dev

或者使用组合命令
```
# 注意：该方案不区分远程仓库是新增还是已有，也不区分本地是否执行过对远程仓库的 pull 或者 fetch 操作

git fetch 远程仓库别名 远程分支名:本地新分支名
git checkout 本地新分支名


# 另一种, 注意：该方案不区分远程仓库是新增还是已有，也不区分本地是否执行过对远程仓库的 pull 或者 fetch 操作

git fetch 远程仓库别名 远程分支名
git checkout -b 本地新分支名 远程仓库别名/远程分支名
```



#### 基于本地分支创建本地分支,并推送到远程分支
```
git branch -c dev d.830 # 基于本地的dev分支创建d.830分支
git branch -D d.830  # 删除本地分支
git checkout d.830  # 切换到d.830分支
git push 
git push --set-upstream origin d.830  # 在远程创建d.830分支

# 删除远程分支（注意：必须在与远程分支关联的本地分支执行）
git push origin --delete d.830
# 这个也可以,推送一个空分支到远程分支，其实就相当于删除远程分支
git push origin :d.830


```


#### 标签操作

- 查看标签
    - `git tag`

- 展示当前分支的最近的tag
    - `git describe --tags --abbrev=0`

- 本地创建标签
    - 默认tag是打在最近的一次commit上
        - `git tag <version-number>`
    - 指定commit打tag
        - `git tag -a <version-numver> -m "v1.0 发布(我是描述)" <commit-id>`


- 推送标签到远程仓库
    - `git push origin <local-version-number>`
    - 一次性推送所有标签到远程仓库
        - `git push origin --tags`


- 删除本地标签
    - `git tag -d <tag-name>`


- 删除远程标签
    - `git push origin :refs/tags/<tag_name>`

- 切回到某个标签
    - `git checkout -b <branch_name> <tag_name>`



#### 切换远程HEAD的指向分支
- git remote set-head origin some_branch


#### 分支重命名
- `git branch -m new_branch_name` 在当前分支时
- `git branch -m old_branch_name new_branch_name` 不在当前分支时


##### 重命名远端分支（假设是在当前分支，并且远端分支与本地分支名是一致的）
- `git branch -m d.830 feature.830`
- `git push --delete origin d.830`
- `git push origin feature.830`
- `git branch --set-upstream-to origin/feature.830`






### git fetch 详解

#### 什么是FETCH_HEAD??

FETCH_HEAD指的是: 某个branch在服务器上的最新状态'.  
每一个执行过fetch操作的项目'都会存在一个FETCH_HEAD列表,   
这个列表保存在 .git/FETCH_HEAD 文件中, 其中每一行对应于远程服务器的一个分支.  
当前分支指向的FETCH_HEAD, 就是这个文件第一行对应的那个分支.  

一般存在两种情况:
- 如果没有显式的指定远程分支, 则远程分支的master将作为默认的FETCH_HEAD.
- 如果指定了远程分支, 就将这个远程分支作为FETCH_HEAD.

常见的 git fetch 使用方式包含以下四种:

- git fetch  
    这一步其实是执行了两个关键操作:
    - 创建并更新所有远程分支的本地远程分支.
    - 设定当前分支的FETCH_HEAD为远程服务器的master分支 (上面说的第一种情况)

需要注意的是: 和push不同, fetch会自动获取远程`新加入'的分支.

- git fetch origin
同上, 只不过手动指定了remote.

- git fetch origin branch1
设定当前分支的 FETCH_HEAD' ~~为远程服务器的branch1分支~~`.

注意: 在这种情况下, 不会在本地创建本地远程分支, 这是因为:

这个操作是 git pull origin branch1 的第一步, 而对应的pull操作,并不会在本地创建新的branch.

一个附加效果是:

这个命令可以用来测试远程主机的远程分支branch1是否存在, 如果存在, 返回0, 如果不存在, 返回128, 抛出一个异常.

- git fetch origin branch1:branch2
只要明白了上面的含义, 这个就很简单了,

首先执行上面的 fetch 操作
使用远程 branch1 分支在本地创建 branch2 (但不会切换到该分支), 
如果本地不存在 branch2 分支, 则会自动创建一个新的 branch2 分支, 
如果本地存在 branch2 分支, 并且是`fast forward', 则自动合并两个分支, 否则, 会阻止以上操作.

- git fetch origin :branch2

等价于: git fetch origin master:branch2


### 给某个分支加 origin
git branch --set-upstream-to=origin/pre pre


-----------------------------------------


- 显示工作区根目录
    - `git rev-parse --show-toplevel`

- 获取分支编号
    - 获取master分支最后commit `git rev-parse master^{commit}`
    - 获取dev分支最后commit `git rev-parse dev^{commit}` 



### git grep 搜索
- 查找包含 'UMP_API' 的代码文件 并 显示所在的行, 要排除 `UMP_API_ALARM_KEY` 这种
    - `git grep -np '\bUMP_API\b'

- 查找包含 'UMP_API' 的代码文件 并 显示所在的行, 要排除 `UMP_API_ALARM_KEY` 这种， 并对结果按文件汇总次数
    - `git grep -np --count '\bUMP_API\b'


### Git 日志搜索
- 找到 UMP_API 常量是什么时候引入的，我们可以使用 -S 选项来显示新增和删除该字符串的提交。
    - `git log -SUMP_API --oneline`




### git archive 相关

- 导出最新的版本库
    - `git archive -o ../latest.zip HEAD`

- 导出指定提交记录
    - `git archive --format=tar -o ../version-1.0.0.tar 9976c24`

- 导出一个目录
    - `git archive --format=zip -o ../version-1.0.0-codes.zip HEAD:src/`

- 导出为tar.gz格式
    - `git archive 9976c24 | gzip > ../version-1.0.0.tar.gz`

- 导出最后一次提交修改过的文件
    - `git archive --format=zip -o ../lastcommit.zip HEAD $(git diff --name-only HEAD^)`




### merge 不同分支
```bash

git co dev  # 切换到 当前分支
git merge pre # 合并 pre 到当前分支
git co -2 engine/handler/appConfigHandler.py  # 对于双方修改的 'engine/handler/appConfigHandler.py' 以当前分支 dev 文件覆盖 
git co -3 engine/handler/appConfigHandler.py  # 对于双方修改的 'engine/handler/appConfigHandler.py' 以当前分支 pre 文件覆盖

# git checkout -2 等价 git checkout -ours
# git checkout -3 等价 git checkout -theirs


# merger 冲突太多撤销
git reset --hard HEAD 

```






