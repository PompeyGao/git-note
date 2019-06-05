## `git` 基本使用

### `git` 安装配置

1.  `Windows` 系统  官网下载安装包，直接安装即可。

2.  `git` 配置

   ```sh
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```

### `git` 仓库创建

​	`git init` 初始化 `git `仓库，自动创建 `master` 分支。

### `git` 基本命令

```sh
   1. touch readme.md  创建 readme 文件。
   2. git status 查看仓库当前状态。
   3. git add readme.md  如果添加的文件比较多，可以直接使用 git add .
   4. git commit -m "add readme file"  提交 readme 文件。
```

### `git` 版本回退

1. 简单版本回退：回到上一个版本 `git reset --hard HEAD^` ; 上上个版本 `HEAD^^` ; 往上100个版本 `HEAD~100` 。

2. 指定 `commit_id` 回退版本：

   通过 `git log` 查看提交记录。如果输出信息太多，可加上 `--oneline ` 参数。

   `git reset --hard <commit_id>` 回退到该次提交的版本。`commit_id` 是由 `git` 自动生成的，通过 `git log` 命令获取。

3. 假如从最新版本回退到了之前的版本，现在后悔了，要再回退到最新版本。但是 `git log` 命令已经获取不到该  `commit_id`，使用 `git reflog` 命令记录每一次的命令，可以找到需要的 `commit_id`。

### 工作区、暂存区、版本库

1. 工作区就是文件所在的文件夹；暂存区在隐藏文件夹`  .git ` 的  `index ` 文件中；` .git `文件夹就是版本库。
2. `git add`把文件从工作区添加到暂存区。
3. `git commit`把暂存区的所有文件提交到版本库当前分支。

### 文件几种状态

​	`git status` 查看文件处于的状态。

1. untracked files

   新增的文件回提示 untracked files，未跟踪的文件意味着git在之前的提交没有这些文件，git 不会自动将之纳入跟踪范围。

2. changes to be committed

   要让 git 管理这些文件，需要运行 `git add` 命令。

   文件状态变为 changes to be committed，说明文件已被跟踪，并处于暂存状态。

3. changes not staged for commit

   修改一个已被跟踪的文件之后，文件状态变为changes not staged for commit，说明已跟踪文件的内容发生了变化，但还没有添加到暂存区。要暂存这次更新，需要运行 `git add` 命令。

### 查看修改

1. 修改但未添加到暂存区（未执行 `git add`）

   - 比较所有修改文件的不同

     `$ git diff`

   - 比较指定修改文件的不同

     `$ git diff <file-name>`

2. 已添加到暂存区但未提交到当前分支（已执行 `git add` 未执行 `git commit`）

   - 比较暂存区与仓库分支（上次 `git commit` ）的不同

     ```sh
     $ git diff --staged
     或
     $ git diff --cached
     ```

### 撤销修改

- 把文件在工作区的修改全部撤销

  `git checkout -- <file-name>`  

  文件修改后还没添加到暂存区，撤销修改后，回到和仓库一样的状态；

  文件修改后已添加到暂存区，之后又作了修改，撤销修改后，回到和暂存区一样的状态。

- 把已经添加到暂存区的修改撤销掉，重新放回工作区

  执行 `git reset HEAD <file-name>` 把文件从暂存区回退到工作区

  再执行 `git checkout -- <file-name>`  对修改进行撤销

- 把暂存区中的所有文件回退到工作区。

  `git reset `   

- 有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交

  `git commit --amend`

- 移除文件

  要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用  `git rm`  命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

  

### 连接远程仓库

1、使用命令 `ssh-keygen -t rsa -C "email@example.com"` 生成 `SSH key` 

2、比如在 `gitlab` 或 `github` 上，上传自己的 `SSH Key`

3、在 `gitlab` 上新建仓库(`repository`)，远程库默认名字为 `origin`，按照页面提示，

- 关联一个全新的库

  ```sh
  git clone ssh://git@gitlab.ecidi.com:10022/xxx/demo.git
  cd demo
  touch README.md
  git add README.md
  git commit -m "add README"
  git push -u origin master
  ```

- 关联已存在文件夹

  ```sh
  cd existing_folder
  git init
  git remote add origin ssh://git@gitlab.ecidi.com:10022/xxx/demo.git
  git add .
  git commit -m "Initial commit"
  git push -u origin master
  ```

- 关联已存在的 `git` 库

  ```sh
  cd existing_repo
  git remote add origin ssh://git@gitlab.ecidi.com:10022/xxx/demo.git
  git push -u origin --all
  git push -u origin --tags
  ```

4、由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，`Git` 不但会把本地的 `master` 分支内容推送到远程新的 `master` 分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

```sh
	$ git pull origin master

​	$ git push origin master
```

5、本地仓库关联多个远程库

```sh
	git remote add github git@github.com:xxx/demo.git //远程库名字为 github  

	git remote add gitlab ssh://git@gitlab.ecidi.com:10022/xxx/demo.git //远程库名为 gitlab

	git remote -v  //查看已配置的远程库信息
```

### 删除已关联的远程库
```sh
	$ git remote rm <remote-name>
```

### 分支创建和合并

```sh
查看分支：$ git branch

创建分支：$ git branch <name>

切换分支：$ git checkout <name>

创建+切换分支：

$ git checkout -b <name> 
或
$ git checkout -b <new_branch> <from_branch>

合并某分支到当前分支：$ git merge <name>

删除分支：$ git branch -d <name>
```

### 解决冲突

1. 使用`git merge`后，可能会出现冲突。

2. 解决冲突后，使用
```sh
   $ git add

   $ git commit
```
   重新进行提交。

### 储藏

```sh
   1. git stash save <message> 将修改储藏起来。
   
      默认情况下，会储藏以下文件：
   
      - 添加到暂存区的修改(staged changes)
      - git 跟踪但并未添加到暂存区的修改(unstaged changes)
   
      不储藏以下文件：
   
      - 在工作区中的新文件(untracked files)
      - 被忽略的文件(ignored files)
   
      git stash save -u <message>  可以 stash  untracked files
   
      git stash save -a <message> 可以 stash 当前目录下所有修改
   
   2. git stash list 查看所有储藏的列表。
   
   3. git stash apply 将储藏的内容恢复，stash 中内容并不删除，使用 git stash drop 来删除。
   
      git stash pop 将最新储藏的内容恢复，同时删除 stash 中内容。
   
      git stash apply stash@{0} 储藏列表中存在多个 stash，恢复指定的 stash
```


### `tag` 标签

​	发布一个版本时，我们通常先在版本库中打一个标签（`tag`），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

```sh
   1. git tag <name> 默认标签会打在最新提交的 commit 上。
   
   2. git tag 查看所有的标签。
   
   3. 若需要在之前的 commit 上打 tag, 先使用 git log --oneline 找到历史提交的 commit id，然后 git tag <name> <commit-id>。
   
   4. 标签不是按时间顺序列出，而是按字母排序的。
   
      可以用 git show <tagname> 查看标签信息。
   
   5. 创建 tag 时，也可以添加说明，用 -a 指定标签名，-m 指定说明文字：
   
      git tag -a <name> -m <message> <commit-id>
   
      > 标签总是和某个 commit 挂钩。如果这个 commit 既出现在 master 分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。
   
   6. git tag -d <name>  删除指定标签。
   
   7. git push origin <tag-name> 单个推送标签到远程
   
      创建的标签都只存储在本地，不会自动推送到远程。
   
   8. git push origin --tags 一次性推送全部尚未推送到远程的本地标签。
   
   9. 删除远程标签的话，先从本地删除：
   
      git tag -d <name>
   
      然后从远程删除：
   
      git push origin :refs/tags/<name>
```

   

### `.gitignore`

​		有些时候，你必须把某些文件放到 `Git` 工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件等等。

​		在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，`Git` 就会自动忽略这些文件。

​		不需要从头写 `.gitignore` 文件，`GitHub` 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[`github/gitignore`](https://github.com/github/gitignore)

- 匹配语法

```sh
# 注释
* 表示匹配0个或多个任意字符
** 表示匹配任意中间目录
/ 以 / 开始，匹配项目根目录；以 / 结束，只匹配文件夹以及在该文件夹路径下的内容
? 匹配一个任意字符
[]  包含单个字符的匹配列表
！表示不忽略(跟踪)匹配到的文件或目录

```

- 示例

  ```
  #               表示此为注释,将被Git忽略
  *.a             表示忽略所有 .a 结尾的文件
  !lib.a          但lib.a除外
  /TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
  build/          表示忽略所有build目录下的所有文件，过滤整个build文件夹
  doc/*.txt       表示会忽略doc/notes.txt，但不包括 doc/server/arch.txt
  
  bin/            表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
  /dir            表示忽略根目录下的任何名为dir的文件、目录、链接等
  /dir/           表示忽略根目录下的dir目录
  /*.c:           表示忽略cat.c，不忽略 build/cat.c
  debug/test.obj: 表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
  **/foo:         表示忽略/foo,a/foo,a/b/foo等
  a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
  !/bin/run.sh    表示不忽略bin目录下的run.sh文件
  *.log:          表示忽略所有 .log 文件
  config.php:     表示忽略当前路径的 config.php 文件
  ```
  
  

### `rebase` 变基

- TODO
  

## 实际开发

1. 两个基本分支 `master` 、`dev` 

2. 每添加一个新功能、修复 `bug`，新建分支进行开发。

   - 新功能开发

     基于 `dev` 分支新建 `feature` 分支，注意，决不应该直接合并到`master`分支。

     ```sh
     1、不使用 pull request/merge request 的方式
     $ git checkout -b feature-some-feature dev
     # 完成功能开发
     $ git add .
     $ git commit -m 'feat: some feature msg'
     $ git pull origin dev
     $ git checkout dev
     $ git merge feature-some-feature
     $ git push origin develop 
     $ git branch -d feature-some-feature
     
     2、使用 pull request/merge request 的方式
     $ git checkout -b feature-some-feature dev
     # 完成功能开发
     $ git add .
     $ git commit -m 'feat: some feature msg'
     $ git push -u origin feature-some-feature
     # 在 GitLab 上发起一个 pull request/merge request
     # 管理员可对代码进行查看，决定是否要合并到 dev 分支，也可以通过 pr/mr 进行讨论
     # 开发人员如果再做修改，同样向上面一样操作，add、commit、push，这次修改内容会同步显示在 pr/mr 上
     # 其他开发人员有需要，可以把 feature-some-feature 分支拉到本地协助修改，修改内容也会同步到 pr/mr 上
     # 管理员接受了pr/mr，会自动对代码进行合并
     # 如果出现冲突，需要在本地解决冲突之后再进行合并和提交。
     # 以 feature-some-feature 分支合并到 dev 分支为例：
     # Step 1. Fetch and check out the branch for this merge request
     $ git fetch origin
     $ git checkout -b feature-some-feature origin/feature-some-feature
     # Step 2. Review the changes locally
     # Step 3. Merge the branch and fix any conflicts that come up
     $ git checkout dev
     $ git merge --no-ff feature-some-feature
     # Step 4. Push the result of the merge to GitLab
     $ git push origin dev
     ```

   - 修复 `bug` 

     ```
     基于 dev 分支新建 fix 分支，按照新功能开发的步骤进行操作
     如果项目已上线，则基于 master 分支新建 fix 分支，将 bug 解决后的 fix 分支合并至 master 分支，然后将 master 分支合并至 dev 分支。
     ```
     注意事项：
     1. 重要分支设置为受保护，杜绝了有些问题代码被提交了，但项目管理员不知道的情况；
     2. 每个任务都有一个对应的分支，互相隔离，所有的代码改动有据可查














1. 每添加一个新功能，最好新建一个 `feature` 分支，在上面开发，完成后，合并，最后删除该 `feature` 分支。

   在 `dev` 分支新建一个 `feature/play_music` 分支：

   - `git checkout -b  feature/play_music`
   - `touch play_music.js`
   - `git add play_music.txt`
   - `git commit -m 'Add play music'`
   - `git checkout dev`
   - `git merge feature/play_music`
   - `git branch -d feature/play_music`     // 非必须进行的一步

   如果删除一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。

2. 多人协作开发

   - 在向远端推送前，先使用 `git pull origin <branch-name>` 拉取对应分支最新的代码
   - 再使用 `git push origin <branch-name>` 推送到远端
   - 如遇冲突，解决冲突后，在进行操作
   - 如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to=origin/<branch-name> <branch-name> `。






> *参考资料：*
>
> *[`Git`工作流指南](http://blog.jobbole.com/76843/)*
>
> [*`Git` 分支管理最佳实践*](https://www.ibm.com/developerworks/cn/java/j-lo-git-mange/index.html)
>
> [*在团队中使用 `GitLab` 中的 `Merge Request` 工作模式*](http://blog.fwhyy.com/2018/06/Use-the-Merge-Request-working-mode-in-GitLab-in-the-team/)