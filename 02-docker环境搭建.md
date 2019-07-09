# 02-docker环境搭建

## 卸载旧版本的docker

`sudo apt-get remove docker docker-engine docker.io containerd runc`

## 更新apt包

`sudo apt-get update`

## 安装所需的apt包

```
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

## 安装GPG证书

`curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -`

## 写入软件源信息

`sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"`

## 列出docker-ce版本

```bash
$ apt-cache madison docker-ce
```

## 安装最新的docker-ce

`sudo apt-get install -y docker-ce docker-ce-cli containerd.io`

查看docker版本信息: `sudo docker version`

## 查看docker的运行状态, 启动docker

```bash
$ systemctl status docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-07-10 00:27:28 CST; 2min 14s ago
     Docs: https://docs.docker.com
 Main PID: 14788 (dockerd)
    Tasks: 13
   CGroup: /system.slice/docker.service
           └─14788 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

$ sudo systemctl start docker

$ sudo systemctl enable docker
```

## 执行docker命令的时候去掉sudo

```bash
# 把docker组添加到系统组里
$ sudo addgroup --system docker

# 把当前用户添加到docker组中
$ sudo adduser $USER docker

# newgrp指令类似login指令, 以相同的帐号, 另一个群组名称,再次登入系统
$ newgrp docker
```

## 修改docker镜像源

```bash
# 如果没有进行任何配置, 下载镜像会超时
$ docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
docker: Get https://registry-1.docker.io/v2/library/hello-world/manifests/sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a: net/http: TLS handshake timeout.
```

在`/etc/docker`目录下创建`daemon.json`文件, 添加以下内容:

```json
{
    "registry-mirrors":[
        "https://kfwkfulq.mirror.aliyuncs.com",
        "https://2lqq34jg.mirror.aliyuncs.com",
        "https://pee6w651.mirror.aliyuncs.com",
        "https://registry.docker-cn.com",
        "http://hub-mirror.c.163.com"
    ],
    "dns":[
        "8.8.8.8",
        "8.8.4.4"
    ]
}
```

重启`docker`: `sudo systemctl restart docker`, 重启之后, 就不会出现下载超时的情况了.

## 参考资料

- [docker配置国内阿里云镜像源](https://blog.csdn.net/m0_37886429/article/details/80323149)
- [ubuntu18.04上安装docker](https://www.jianshu.com/p/3ff8913a5934)
- [Docker CE 镜像源站](https://yq.aliyun.com/articles/110806)
