---
title: "Docker 安装 HDDM"
short_title: "Docker 安装 HDDM"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# Docker 安装 HDDM

Created: June 11, 2023 9:26 AM
Last edited by: Pan Wanke
Last edited time: June 11, 2023 9:26 AM
Owner: Pan Wanke

[HDDM 安装教程.html](Docker%20%E5%AE%89%E8%A3%85%20HDDM/HDDM_%25E5%25AE%2589%25E8%25A3%2585%25E6%2595%2599%25E7%25A8%258B.html)

### docker 安装

首先，安装 docker

下载 docker，有 600M [Docker: Accelerated, Containerized Application Development](https://www.docker.com/)

[[HDDM 安装教程_.png]]

[[HDDM 安装教程__1.png]] 注意：Windows 10 版本 2004 或更高版本才能使用 wsl2。 可以在命令行中使用 winver 查看版本号。 如果版本低于 2004，请不要勾选 wsl2，此时可以使用 Hyper-V 来运行 docker 环境。 相关教程可以看如下链接，如果实在不知道如何安装欢迎留言。 [[Pasted image 20230408082627.png]]

[[HDDM 安装教程__2.png]] 由于只能装 C 盘，所以直接点 “ok” 就开始安装了。 大概安装 2-3分钟

如果提醒 “log out”，就需要注销电脑，请注意。 [[HDDM 安装教程__3.png]]

注销或重启后，点击“接受”协议即可。 [[Pasted image 20230407233744.png]]

## wsl2 与 linux(Ubuntu) 安装—可选

### 安装 wsl2

此时，可能提醒需要更新或安装 wsl。 [[Pasted image 20230408081205.png]]

可以通过命令行查看 wsl 情况，输入 `wsl --list --verbose` [[Pasted image 20230408084551.png]]

如果提示不存在 wsl 分发版。那我们需要安装 wsl 发行版。 首先，以管理员身份打开命令行，启用“虚拟机平台”可选功能，然后才能在 Windows 上安装 Linux 分发。 - 运行以下命令以启用虚拟机平台功能： `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` - 运行以下命令以安装WSL 2更新： `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart` [[Pasted image 20230408083421.png]]

- 下载并安装WSL 2 Linux内核更新包：[https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4—download-the-linux-kernel-update-package](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
- 运行以下命令以将WSL 2设置为默认版本：
    
    ```
    wsl --set-default-version 2
    ```
    
    [[Pasted image 20230408084531.png]]
    
- 此时，可以重启，使得 wsl2 生效 (可选)。

### 安装 linux(Ubuntu)

### 通过 wsl2 安装

`wsl --install` 是一个新的命令，用于自动安装WSL2和Ubuntu Linux发行版，以及安装和配置Windows Terminal和其他必要的组件。该命令旨在简化安装和配置WSL2和Linux发行版的过程，并使其更加用户友好。 运行`wsl --install`命令将自动下载和安装WSL2和Ubuntu Linux发行版，并将其配置为默认发行版。此外，它还将下载和安装Windows Terminal、Windows Subsystem for Linux更新程序和其他必要的组件，以确保您可以顺利使用WSL2和Linux发行版。 请注意，运行此命令需要管理员权限，并且可能需要一些时间来下载和安装所有必要的组件。如果您已经安装了WSL2和Linux发行版，则不需要运行此命令，因为它可能会覆盖您的现有设置和配置。 也可以通过 `wsl --install -d Ubuntu-22.04` 选择系统版本。其中，Ubuntu-22.04，Ubuntu-20.04 都是推荐的。 [[Pasted image 20230408093646.png]]

安装后，启动系统： - `wsl -d Ubuntu-22.04` 如果失败可以尝试以下命令，但需要等待几分钟。 - `wsl --set-version Ubuntu-22.04 2 && wsl -d Ubuntu-22.04`

### 通过 Microsoft Store 进行安装

如果觉得代码麻烦，也可以尝试使用 Microsoft Store 进行安装 - 打开Microsoft Store，选择合适的Linux分发版，进行安装。这里选择免费的 Ubuntu 系统就可以了。 [[Pasted image 20230408084705.png]]

[[Pasted image 20230408084739.png]]

### 查看并进入系统

通过 `wsl --list --verbose` 或 `wsl -l -v` 查看安装的系统 [[Pasted image 20230408085623.png]]

安装后打开系统进行基本设置，设置系统用户名和密码。注意：输入的密码不会显示出来。 [[Pasted image 20230408090658.png]]

### 迁移出C盘 (可选)

由于 linux 子系统默认安装在 C 盘，随着后面系统更新，以及在子系统中安装各种软件会占据大量空间。 如果有需要，大家可以把该系统移动到其他盘符。 首先，导出该系统到其他盘符并存储为 .tar格式 ，如 D - `wsl --export Ubuntu-22.04 D:\wsl-Ubuntu-22.04.tar` [[Pasted image 20230408090938.png]] 接着，注销系统 - `wsl --shutdown` - `wsl --unregister Ubuntu-22.04` [[Pasted image 20230408091001.png]] 重新导入并安装WSL在新地址中(当然地址可以自行选择)，如 `d:\wsl2_ubuntu` - 具体命令： `wsl --import Ubuntu-22.04 d:\wsl2_ubuntu d:\wsl-Ubuntu-22.04.tar --version 2` [[Pasted image 20230408091418.png]]

启动系统 [[Pasted image 20230408092144.png]] - 或者使用命令：`wsl --start Ubuntu-22.04` 或者 `wsl -d Ubuntu-22.04` 或者 `wsl --set-version Ubuntu-22.04 2 && wsl -d Ubuntu-22.04`

此时，可以删除tar文件(可选) `del d:\wsl-ubuntu20.04.tar` 注意，如果之前从未重启，建议先重启….

### Ubuntu 系统升级 + 安装软件包

更换国内源 首先，启动系统 `wsl -d Ubuntu-22.04` [[Pasted image 20230408103115.png]] - 将系统源文件复制备用 - `sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak` - 用 vi 编辑器打开源文件， - `sudo vi /etc/apt/sources.list` - 然后直接输入dd清除所以内容，然后粘贴以下内容

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

[[Pasted image 20230408103214.png]] - 然后，按下 esc，:wq 保存并退出

更新系统 - `sudo apt-get -y update && sudo apt-get -y upgrade` [[Pasted image 20230408103325.png]]

## docker 设置 + 安装 HDDM

在设置->安装后选择启用WSL2引擎 [[Pasted image 20230408104919.png]]

### 可选：配置数据和镜像源的存放位置 + 镜像源

配置数据和镜像源的存放位置 + 镜像源, 这里配置的官方镜像，大家可以自行配置 - 也可在 `C:\Users\<UserName>\.docker\daemon.json` 中进行设置 - 其中，`"data-root":"d\\docker"` 设定了数据的存放位置。 - `"registry-mirrors"` 设置了镜像源

```
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "features": {
    "buildkit": true
  },
  "data-root":"d\\docker",
  "registry-mirrors": [
      "https://registry.docker-cn.com"
  ]
}
```

[[Pasted image 20230408122614.png]]

### 可选：移除 C 盘

```
wsl --export docker-desktop D:\docker\docker-desktop.tar
wsl --export docker-desktop-data D:\docker\docker-desktop-data.tar
wsl --export Ubuntu-22.04 D:\docker\Ubuntu-22.04.tar

wsl --unregister docker-desktop
wsl --unregister docker-desktop-data
wsl --unregister Ubuntu-22.04

wsl --import docker-desktop D:\docker\docker-desktop D:\docker\docker-desktop.tar --version 2
wsl --import docker-desktop-data D:\docker\docker-desktop-data D:\docker\docker-desktop-data.tar --version 2
wsl --import Ubuntu-22.04 D:\Ubuntu_2204 D:\docker\Ubuntu-22.04.tar --version 2
```

### 正式安装 HDDM

之后就可以在wsl命令行中使用docker了。 找到 HDDM docker [[Pasted image 20230408105556.png]] - 可直接点解 pull - 也可以运行命令 `docker pull hcp4715/hddm -o /path/to/my/images` - 其中，-o 可以设置储存的**相对位置**

### docker 启动 HDDM 镜像

```
# windows
docker run -it --rm --cpus=4 -v /d/hcp4715/hddm_docker:/home/jovyan/work -p 8888:8888 hcp4715/hddm jupyter notebook

# Ubantu
docker run -it --rm --cpus=4 \
-v /home/hcp4715/hddm_docker:/home/jovyan/work \
-p 8888:8888 hcp4715/hddm:0.8 jupyter notebook
```

[[HDDM 安装教程__5.png]]

也可以通过界面操作启动 [[Pasted image 20230408130513.png]] [[Pasted image 20230408130531.png]]

新建一个 notebook [[Pasted image 20230408131406.png]]

导入 hddm 进行测试 [[Pasted image 20230408131654.png]]
