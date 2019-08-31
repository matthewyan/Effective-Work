# git

#### alias配置
打开或新建.gitconfig文件：`vi ~/.gitconfig`  ([配置别名](https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424))

加上如下内容:
``` sh
[alias]
        st = status
        co = checkout
        ci = commit
        br = branch
        lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```
此时就可以使用 git ci -m "msg" 来提交代码了