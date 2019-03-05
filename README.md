#1

###git仓库的创建

1. `git init` 初始化git仓库.
2. `touch readme.md`  创建readme文件.
3. `git status` 查看仓库当前文件的状态.
4. `git add readme.md`  如果添加的文件比较多，可以直接使用`git add .`
5. `git commit -m "add readme file"`  提交`readme`文件.

###git版本回退

1. `git log`查看提交记录.
2. `git reset --hard <commit_id>` `commit_id`是由git自动生成的，通过`git log`命令获取.
3. 假如从最新版本回退到了之前的版本，现在后悔了，要再回退到最新版本。但是`git log`命令已经获取不到.该`commit_id`，使用`git reflog`命令记录每一次的命令，可以找到需要的`commit_id`。

###连接远程仓库

