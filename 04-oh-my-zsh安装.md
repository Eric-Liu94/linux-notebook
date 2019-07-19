# oh-my-zsh安装

时间: `2019-07-13`

## 安装zsh

首先查看当前`Linux`系统支持哪些`shell`: 

```bash
$ cat /etc/shells

# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash

```

安装`oh-my-zsh`之前, 必须安装`zsh`, 所以需要安装一下`zsh`: 

```bash
$ sudo apt-get install -y zsh
$ cat /etc/shells

# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/bin/zsh
/usr/bin/zsh

# change shell, 将shell环境真正切换到zsh
$ sudo chsh -s /usr/bin/zsh

# 此时打印shell环境变量, 发现还是默认的bash, 而不是zsh, 这里一定要退出重新登录一下
$ echo $SHELL
/bin/bash
```

## 安装Oh My Zsh

安装之前首先需要安装`git`: `sudo apt-get install -y git`

执行官方提供的命令:
 
`sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`


### https://www.jianshu.com/p/4fde9ae77922