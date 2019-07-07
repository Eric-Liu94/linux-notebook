# ubuntu linux使用

时间: `2019-07-05`
版本: `ubuntu 18.04.2 LTS`

## 拨号上网

```
$ sudo pppoeconf
# 经过一系列的设置之后开始拨号
$ pon dsl-provider
```

## 解决未发现WiFi适配器

```
# 1. 插网线
# 2. 打开软件和更新 -> Ubuntu软件 -> 下载自 -> http://mirrors.aliyun.com/ubuntu
# 3. 更新软件
$ sudo apt-get update
# 4. 安装驱动
$ sudo apt-get install bcmwl-kernel-source
```

## 关闭笔记本的内置键盘

执行命令: `xinput list`, 可以看到以下结果：

```
$ xinput list
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ USB OPTICAL MOUSE                       	id=10	[slave  pointer  (2)]
⎜   ↳ SynPS/2 Synaptics TouchPad              	id=13	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Power Button                            	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Video Bus                               	id=8	[slave  keyboard (3)]
    ↳ Power Button                            	id=9	[slave  keyboard (3)]
    ↳ HP Truevision HD: HP Truevision         	id=11	[slave  keyboard (3)]
    ↳ HP WMI hotkeys                          	id=14	[slave  keyboard (3)]
    ↳ HP Wireless hotkeys                     	id=15	[slave  keyboard (3)]
    ↳ Heng Yu Technology Poker System Control 	id=16	[slave  keyboard (3)]
    ↳ Heng Yu Technology Poker Consumer Control	id=17	[slave  keyboard (3)]
    ↳ Heng Yu Technology Poker                	id=18	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=12	[slave  keyboard (3)]
```

上面列出的`AT Translated Set 2 keyboard`就是笔记本的内置键盘, 查询到它所对应的`id`, 然后禁用：

```
$ xinput set-prop 12 "Device Enabled" 0
```

## Caps Lock键代替Ctrl键

`$ setxkbmap -option ctrl:nocaps`

## 安装配置emacs

```
$ sudo apt-get install -y emacs
```

emacs编辑文件总是会在当前目录生成一个临时文件, 例如: `filename~`, 为了消除这个文件, 需要在`~/.emacs.d`目录下创建`init.el`文件, 加入以下内容:

```
;; all backups goto ~/.backups instead in the current directory
(setq backup-directory-alist (quote (("." . "~/.backups"))))
```

## 安装shadowsocks

```
$ sudo apt-get install shadowsocks
```

复制出一个新的配置文件:

```
sudo cp /etc/shadowsocks/config.json /etc/shadowsocks/cli-config.json
```

编辑`cli-config.json`文件的内容: 

```
{
    "server": "xxx.xxx.xxx.xxx",
    "server_port":xxxx,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"******",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1,
    "prefer_ipv6": false
}
```

启动`shadowsocks客户端`:

```
sslocal -c /etc/shadowsocks/cli-config.json
```

此时浏览器还无法走代理的网络, 需要配置网络:

```
# 以火狐浏览器为例
# 1. 首选项 -> 网络设置(页面最下面) -> 设置
# 2. 手动代理配置 -> SOCKS主机和端口 -> 127.0.0.1:1080 -> SOCKS_v5协议
# 3. 使用SOCKS v5时代理DNS查询
```

## github ssh配置

在终端生成公钥和私钥:

```bash
ssh-keygen -t rsa -b 4096 -C "xxx@gmail.com"
```

在`~/.ssh`目录中创建`config`文件, 并添加以下内容:

```
# github
Host github.com
     HostName github.com
     IdentityFile ~/.ssh/id_rsa
```

把生成的公钥`id_rsa.pub`文件的内容复制到`github`中, 最后在终端测试一下:

```bash
ssh -T git@github.com
```

> 提示以下类似信息证明配置成功: Hi Eric-Liu94! You've successfully authenticated, but GitHub does not provide shell access.


