###git仓库的创建

1. `git init` 初始化git仓库，自动创建`master`分支。
2. `touch readme.md`  创建readme文件.
3. `git status` 查看仓库当前文件的状态.
4. `git add readme.md`  如果添加的文件比较多，可以直接使用`git add .`
5. `git commit -m "add readme file"`  提交`readme`文件.

###git版本回退

1. `git log`查看提交记录.
2. `git reset --hard <commit_id>` `commit_id`是由git自动生成的，通过`git log`命令获取.
3. 假如从最新版本回退到了之前的版本，现在后悔了，要再回退到最新版本。但是`git log`命令已经获取不到.该`commit_id`，使用`git reflog`命令记录每一次的命令，可以找到需要的`commit_id`。

### 工作区和暂存区

1. 工作区就是文件所在的文件夹，暂存区在隐藏文件夹`.git`中。
2. `git add`把文件从工作区添加到暂存区。
3. `git commit`把暂存区的所有文件提交到当前分支。

###撤销修改

`git checkout -- <file>`  把文件在工作区的修改全部撤销

1. 文件修改后还没添加到暂存区，撤销修改后，回到和仓库一样的状态；
2. 文件修改后已添加到暂存区，之后又作了修改，撤销修改后，回到和暂存区一样的状态。

`git reset HEAD <file>`把已经添加到暂存区的修改撤销掉，重新放回工作区。

###连接远程仓库

1、生成`SSH key`

2、比如在`gitlab`上，上传自己的`SSH Key`

3、新建仓库(`repository`)，按照页面提示，使用

​	`git remote add origin ssh://git@ecgitlab.ecidi.com:10022/pompeygao/demo.git`命令，将本地与远程仓库关联。

​	`git push -u origin master`把本地代码推送到远程库上。

4、由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

​	`git pull origin master` 

​	`git push origin master`

### 分支创建和合并

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 解决冲突

1. 使用`git merge`后，可能会出现冲突。
2. 先解决冲突，再使用`git add     git commit`命令进行提交。

### 储藏

1. `git stash save <message>` 将修改储藏起来。

   默认情况下，会储藏以下文件：

   - 添加到暂存区的修改(staged changes)
   - git跟踪单并未添加到暂存区的修改(unstaged changes)

   不储藏以下文件：

   - 在工作区中的新文件(untracked files)
   - 被忽略的文件(ignored files)

   `git stash save -u <message>`  可以 stash  untracked files

   `git stash save -a <message>` 可以 stash 当前目录下所有修改

2. `git stash list` 储藏的列表。

3. `git stash apply`将储藏的内容恢复，`stash`中内容并不删除，使用`git stash drop`来删除。

   `git stash pop`将储藏的内容回访，同时删除`stash`中内容。

   `git stash apply stash@{0}`储藏列表中存在多个`stash`，恢复指定的`stash`。