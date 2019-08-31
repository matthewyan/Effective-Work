# git

#### 重置相关

| 命令                               | 作用               |
| ---------------------------------- | ------------------ |
| `git checkout .`                   | 丢弃所有文件的修改 |
| `git checkout -- file`             | 丢弃单个文件的修改 |
| `git clean -xdf`                   | 丢弃所有新增的文件 |
| `git clean -f a.txt`               | 移除单个新增的文件 |
| `git checkout . && git clean -xdf` | 丢弃所有修改       |



----

#### alias配置

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