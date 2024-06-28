---
title: 重置 Windows 并且安装 Ubuntu 双系统
---

## 1. [可选] 重置 Windows

先把重要的东西备份好.

根据下图步骤重置你的 Windows.

<img src="imgs/reset_windows.png"></img>


注意, 在 Step 8, 如果你有多个磁盘, 建议把别的磁盘也给格式化了.

## 2. 下载 Ubuntu Desktop

进入 https://ubuntu.com/download/desktop .

Ubuntu 23 比较好看, 所以接下来我准备下载 23. 你也可以选择下载 22.

官方源不翻墙下载太慢了. 推荐从镜像源下载.

向下滑动页面, 找到并点击下图中的 "see our alternative downloads".

<img src="imgs/see_out_alter_downloads.png"></img>


然后根据下图翻翻翻, 点点点, 下载 "ubuntu-23.10.1-desktop-amd64.iso".

<img src="imgs/download_ubuntu_iso.png"></img>

选择从南京大学的镜像站下载是因为我自己用下来感觉最快. 你可以根据自己的网络环境选择其他的镜像站.

## 3. 制作启动盘

准备一个大于 16G 的闲置U盘. 等会U盘会被格式化, 所以里面别放重要的东西.

下载 Rufus, 网址: https://rufus.ie/en/ .

用起来很简单, 选择你的U盘, 选择前面下载好的 iso, 点 START.

## 4. 装双系统

### 4.1. 看自己有多少磁盘

<img src="imgs/disk_manager.png"></img>

### 4.2. 看自己的内存大小

<img src="imgs/check_ram.png"></img>

可以看到我的电脑 RAM 是 16 GB.

### 4.3. 情况 1: 对于一个磁盘, 希望一部分用于 Windows, 另一部分分配给 Ubuntu (以 C 盘为例)

#### 4.3.1. 压缩目标磁盘

根据下图, 先压缩目标磁盘, 预留出足够空闲空间给 Ubuntu:

<img src="imgs/shrink_disk.png"></img>

> 💡 如果你的 SSD 明明有很多容量, 但是能够 shrink 的部分很小 (比如 475G 只能 shrink 100G), 强烈建议重装一下 Windows.

#### 4.3.2. 进入安装引导

将 [3. 制作启动盘](#3-制作启动盘) 中制作好的启动盘插入电脑.

电脑关机; 再按电源键开机立刻疯狂按 F2 进入 Bios 模式.

然后你需要想办法更改启动选项为 USB (也就是刚刚做的启动盘).

不同电脑的更改方式不一样, 比如联想的电脑可以参考下图:

<img src="imgs/lenovo_boot.png"></img>

惠普的电脑, 先要选择退出 Hardware Diagnostics UEFI, 然后选择启动选项:

<img src="imgs/hp_boot.png"></img>

> 📌 注意: 启动时, 如果系统问你选择用什么 boot 模式, 记得选 normal mode 而非 grub2 mode.

#### 4.3.3 开始安装 Ubuntu

安装参考下面的步骤 (不同版本 Ubuntu 可能顺序不一样, 不过大差不差):

<img src="imgs/install_ubuntu_1.png"></img>

### 4.4. 情况 2: 有一个独立磁盘安装 Ubuntu

#### 4.4.1. 释放目标磁盘

参考下图做以下几件事情: 
1. Win + R 打开 Run.
2. 输入 "Diskpart".
3. 输入命令:
    ```bash
    list disk
    ```
    **注意: 不要释放掉安装了 Windows 的磁盘.**
4. 找到额外的磁盘, 比如我希望把 Linux 安装在 Disk 1 中, 就执行下面两条命令把 Disk 1 释放掉:
    ```bash
    select disk 1  # Choose the disk where you want to install Linux.
    clean  # "Clean" the disk, aka. free and format the disk.
    ```

<img src="imgs/clean_disk.png"></img>

#### 4.4.2. 进入安装引导

与 [4.3.2. 进入安装引导](#432-进入安装引导) 中的步骤一样.

#### 4.4.3. 开始安装 Linux

安装参考下面的步骤 (不同版本 Ubuntu 可能顺序不一样, 不过大差不差):

<img src="imgs/install_ubuntu_2.png"></img>

## 5. 修复启动引导

安装完重启 ubuntu, 会进入启动引导界面, 直接选择进入 ubuntu.

进入后 ctrl+alt+t 调出 terminal, 输入以下两条命令:

```bash
sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt update

sudo apt install -y boot-repair && boot-repair
```

弹出的窗口全选确认, 等待修复完成即可.

## 6. 简单个性化和开发环境配置

### 6.1. 安装 VIM

下载 `vim-gtk3`, 将原始配置文件重命名为 `vimrc.bak`, 然后新建 `vimrc` 文件并写入我们提供的 [vimrc](./vimrc) 内容 (按 `i` 进入 INSERT 模式, 按 `Ctrl-Shift-v` 粘贴, 按 `ESC` 进入 NORMAL 模式, 按 `:` 进入 COMMAND 模式, 输入 `wq` 后按 `ENTER` 保存退出)

```bash
sudo apt-get install vim-gtk3
sudo mv /etc/vim/vimrc /etc/vim/vimrc.bak
sudo vim /etc/vim/vimrc  # 写入我们提供的 vimrc 内容
```

如果你不会使用 vim, 安装完后, 请运行下面的指令开始 30 分钟的 vim 速成:

```bash
vimtutor
```

### 6.2. [可选] 安装 Nvidia 驱动和 CUDA TOOLKIT (需要你的电脑有 N 卡)

如果你电脑没有 N 卡自然跑不了 Pytorch 捏~

本文选择安装了 535 版本的驱动, 目前测试没有 bug. 你可以结合自己的情况试试安装更新的驱动, 不过版本一定要记住, 后面安装 cuda tookit 要用到.

参考下图安装驱动:

<img src="imgs/nvidia_driver.png"></img>

安装完后重启一下, 接着我们继续安装 cuda toolkit.

先安装 Build Essentials, 也就是 gcc, g++, gdb:

```bash
sudo apt install build-essential
```

进入 [Cuda Tookit Archive 官网](https://developer.nvidia.com/cuda-toolkit-archive).

然后要注意, 当前最新 (2024/Feb/09) 的 cuda toolkit 是 12.3.2 (January 2024), 点进去选好操作系统 (Linux => x86_64 => Ubuntu => 22.04 => runfile) 后可以看到下载指令.

**但是我们前面安装的是 535 版本的驱动, 上述指令下载的对应驱动是 545 版本. 因此出现了不匹配的状况.**

因此你在下载时, **需要结合你安装的驱动版本找到最新的与驱动版本匹配的 cuda toolkit**. 

> 💡 你可以在系统中安装多个 cuda toolkit 版本, 只要系统驱动版本高于 toolkit 要求的版本即可.

对于 535 版本的驱动, 最新的匹配 cuda toolkit 应该是 [12.2.2 (August 2023)](https://developer.nvidia.com/cuda-12-2-2-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local).

点进去后选择操作系统 (虽然我们安装的是 Ubuntu 23, 但是 22.04 的 Cuda Tookit 也能装):

Linux => x86_64 => Ubuntu => 22.04 => runfile(local)

可以看到跳出下面的两条指令; 第一条是下载 cuda tookit 安装程序, 第二条是安装 cuda tookit:

```bash
# [Warning] Following commands are just an example. You should get your command from the offical website: https://developer.nvidia.com/cuda-toolkit-archive

# Download cuda toolkit
wget https://developer.download.nvidia.com/compute/cuda/12.2.2/local_installers/cuda_12.2.2_535.104.05_linux.run
# Run installation script
sudo sh cuda_12.2.2_535.104.05_linux.run
```

参考下图的步骤进行 cuda toolkit 的安装 (注意取消安装 Driver):

<img src="imgs/install_cuda_toolkit.png"></img>

安装完后, 根据输出的提示, 利用指令 `sudo vim /etc/bash.bashrc` 在文件的末尾添加以下两行命令 (具体命令要你参考下图根据输出提示改, 不一定和我的一样):

```bash
export PATH="$PATH:/usr/local/cuda-12.2/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda-12.2/lib64"
```

<img src="imgs/install_cuda_toolkit_2.png"></img>

> 💡 如果安装了多个版本的 cuda toolkit, 可以通过更改上面的路径并重启终端实现切换 cuda toolkit 版本.

安装完后, 把当前终端关闭, 开个新终端, 输入以下指令检查 cuda toolkit 是否安装成功:

```bash
nvcc -V
```
