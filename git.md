# git

### 重置相关

##### 还未添加到暂存区
| 命令                               | 作用               |
| ---------------------------------- | ------------------ |
| `git checkout .`                   | 丢弃所有文件的修改 |
| `git checkout -- file`             | 丢弃单个文件的修改 |
| `git clean -xdf`                   | 丢弃所有新增的文件 |
| `git clean -f a.txt`               | 移除单个新增的文件 |
| `git checkout . && git clean -xdf` | 丢弃所有修改       |

##### 已添加到暂存区，但还未提交
| 命令                    | 作用             |
| ----------------------- | ---------------- |
| `git reset HEAD`        | 将修改移回工作区 |
| `git reset --hard HEAD` | 将修改丢弃       |

##### 已经commit，但还未push

| 命令                     | 作用                                |
| ------------------------ | ----------------------------------- |
| `git reset --hard HEAD^` | 跳到上个结点 (HEAD^^跳到上上个结点) |

##### 已经push到远端的提交

| 命令                  | 作用                                |
| --------------------- | ----------------------------------- |
| `git revert commitID` | 重置某次提交 (处理完`git push`即可) |
| `git revert HEAD`     | 将最新的提交revert掉                |

还有一种不安全的方式：`git reset --hard commitID` 然后 `git push -f`。 表示先在本地重置到某次提交，然后强制将远端的记录用本地替换。



----

​	

### 分支相关

| 命令                          | 作用                                                       |
| ----------------------------- | ---------------------------------------------------------- |
| `git checkout master`         | 切换到本地已有的分支                                       |
| `git branch name`             | 从当前分支创建一个新分支，但不切换                         |
| `git branch -a`               | 查看所有分支，包括远端分支                                 |
| `git branch -v`               | 查看本地分支跟踪的远程分支信息                             |
| `git branch -u=origin/master` | 设置远程分支跟踪 (`-u`与`--set-upstream-to`等同)           |
| `git merge --no-ff develop`   | 在merge指定分支时，生成一个合并结点 (即禁用`Fast-Forward`) |



----

​	

### 冲突相关

在merge冲突后，修复完冲突，需要执行如下指令：

``` shell
1. git add *		# 将修改添加
2. git commit -m *	# 将修改提交
3. git merge --continue	# 继续执行流程
```

如果不想继续解决冲突，则：

```shell
git merge --abort
```

rebase冲突后，类似

``` shell
git rebase --continue	# 继续执行流程
git rebase --abort	# 中止rebase
```



在冲突解决时，要注意ours和theirs的含义：

> merge时，ours表示当前分支，theirs表示被合并分支
>
> rebase时，theirs表示当前分支，ours是表示被rebase的分支

参考：[化解冲突：git merge 与 git rebase 中的 ours 和 theirs](https://bitmingw.com/2017/02/16/git-merge-rebase-ours-and-theirs/)



----

​	

### SubModule

| 命令                                              | 作用                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| `git submodule init`                              | 初始化工程 (新clone的代码，要更新submodule，也需要调用这个命令) |
| `git submodule update`                            | 更新submodule代码                                            |
| `git submodule add https://gitrepo.git directory` | 添加submodule到当前工程                                      |
| `git push origin HEAD:master`                     | 推送修改的代码至远端仓库指定分支                             |

submodule相关信息，存储在了`.gitmodules`文件中

要删除某个submodule，参考：[delete_git_submodule.md](https://gist.github.com/myusuf3/7f645819ded92bda6677)



----

​	

### SubTree

| 命令                                                         | 作用        |
| ------------------------------------------------------------ | ----------- |
| `git subtree add --prefix=Path http://gitrepo.git master --squash` | 添加subtree |
| `git subtree pull --prefix=Path http://gitrepo.git master --squash` | 更新代码    |
| `git subtree push --prefix=Path http://gitrepo.git master`   | 推送代码    |

要查看subtree仓库有哪些，可以使用：

``` shell
git log | grep git-subtree-dir | tr -d ' ' | cut -d ":" -f2 | sort | uniq
```



----

​	

### 其它

| 命令                                                | 作用                                         |
| --------------------------------------------------- | -------------------------------------------- |
| `git reflog`                                        | 查看有执行的记录                             |
| `git show`                                          | 查看最近一次提交的修改                       |
| `git show commitID`                                 | 查看指定commitID的修改                       |
| `git show commit file`                              | 查看某次，某个文件的修改                     |
| `git fetch --all && git reset --hard origin/master` | 重置本地所有修改，并更新到远端仓库的最新代码 |



----

​	

### alias配置

打开或新建.gitconfig文件：`vi ~/.gitconfig`  ([配置别名](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424))

加上如下内容:
``` sh
[alias]
        st = status
        co = checkout
        ci = commit
        br = branch
        pr = pull -r
        lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
        ls-subtrees = !"git log | grep git-subtree-dir | tr -d ' ' | cut -d ":" -f2 | sort | uniq"
```
此时就可以使用 git ci -m "msg" 来提交代码了