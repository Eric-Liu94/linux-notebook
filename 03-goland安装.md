# 03-goland安装

时间: `2019-07-13`

## 版本信息

- goland: `2019.1.3`

## 安装goland

```bash
$ sudo tar -zxzf goland-2019.1.3.tar.gz -C /opt
$ sudo ln -s /opt/GoLand-2019.1.3 /opt/goland

$ cd /opt/goland
$ ./bin/goland.sh
```

## 正常使用

准备文件: 
- `agent.jar`
- `activation-code.txt`

操作开始: 

- 把`agent.jar`放到`/opt/goland/bin`目录下 
- 找到`/opt/goland/bin/goland64.vmoptions`(如果是32位系统, 编辑的是`goland.vmoptions`文件)
- 编辑在`goland64.vmoptions`文件的最后, 添加这样一条信息`-javaagent:/opt/goland/bin/agent.jar` 
- 打开软件: `./goland.sh`
- 选择`Activate -> Activation code`, 复制`activation-code.txt`文件的内容到文本框中, 点击按钮`Activate`即可

为了方便在终端快速启动goland, 创建命令行工具: 

`菜单Tools -> Create Command-line Launcher...`

创建完成后, 进入到项目目录下, 直接运行`goland .`即可用goland打开当前项目.

## 解决错误

打开软件的时候, 可能会提示这样的错误:

```
Gtk-Message: 15:19:45.367: Failed to load module "canberra-gtk-module"
```

安装一下这个包即可:

`sudo apt-get install libcanberra-gtk-module`

