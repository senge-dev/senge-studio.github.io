# ArchLinux安装

## 验证自己的计算机是否满足安装Arch Linux的要求

确保自己的计算机支持UEFI启动

确保自己的CPU使用的是x86_64架构

如果不能同时满足以上两种要求，请勿参考此文档，不然作者不能保证安装成功。

## 根据自己的需求选择合适的安装方式

<!-- tabs:start -->

<!-- tab:下载ArchISO -->

你应该下载Arch Linux ISO镜像文件的几种情况：

- 你想使用常规方法安装Arch Linux
- 你想使用ssh远程连接的方法安装Arch Linux

### 从合适的镜像站下载Arch Linux ISO镜像文件

| 镜像站                     | 下载地址                                                   |                          哈希值校验                          |
| :------------------------- | :--------------------------------------------------------- | :----------------------------------------------------------: |
| 清华大学开源镜像站         | https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/ | [校验地址](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| 阿里云开源镜像站           | https://mirrors.aliyun.com/archlinux/iso/latest/           | [校验地址](https://mirrors.aliyun.com/archlinux/iso/latest/sha256sums.txt) |
| 北京外国语大学开源镜像站   | https://mirrors.bfsu.edu.cn/archlinux/iso/latest           | [校验地址](https://mirrors.bfsu.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| 中国科学技术大学开源镜像站 | https://mirrors.ustc.edu.cn/archlinux/iso/latest/          | [校验地址](https://mirrors.ustc.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| Arch Linux官网             | https://archlinux.org/download/                            |                              ——                              |

> **注意**: 无论从哪个镜像站下载，请务必验证镜像的`sha256`哈希值，确保下载的archlinux iso没有被恶意修改。

### sha256验证

- 在Microsoft Windows或在PowerSheel中进行sha256校验

按下`Win`+`X`，然后按`A`打开`PowerShell`，然后运行以下命令

```powershell
Get-FileHash C:\Users\Administrator\Download\archlinux-2022.05.01-x86_64.iso
```

- 在Linux或在macOS中进行sha256校验

Linux系统使用`Ctrl`+`Alt`+`T`打开终端，macOS用户使用`Command`+`Space`，然后搜索`Terminal`打开终端

```bash
sha256sum $HOME/Downloads/archlinux-2022.05.01-x86_64.iso
```

<!-- tab:下载ArchLinux Bootstrap -->

你应该下载Arch Linux Bootstrap的几种情况：

- 你想在其他Linux发行版本中安装Arch Linux
- 你想将Arch Linux安装在Windows Subsystem for Linux中

### 从合适的镜像站下载ArchLinux Bootstrap

| 镜像站                     | 下载地址                                                   |                          哈希值校验                          |
| :------------------------- | :--------------------------------------------------------- | :----------------------------------------------------------: |
| 清华大学开源镜像站         | https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/ | [校验地址](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| 阿里云开源镜像站           | https://mirrors.aliyun.com/archlinux/iso/latest/           | [校验地址](https://mirrors.aliyun.com/archlinux/iso/latest/sha256sums.txt) |
| 北京外国语大学开源镜像站   | https://mirrors.bfsu.edu.cn/archlinux/iso/latest           | [校验地址](https://mirrors.bfsu.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| 中国科学技术大学开源镜像站 | https://mirrors.ustc.edu.cn/archlinux/iso/latest/          | [校验地址](https://mirrors.ustc.edu.cn/archlinux/iso/latest/sha256sums.txt) |
| Arch Linux官网             | https://archlinux.org/download/                            |                              ——                              |

> **注意**: 无论从哪个镜像站下载，请务必验证镜像的`sha256`哈希值，确保下载的archlinux Bootstrap没有被恶意修改。

### sha256验证

- 在Microsoft Windows或在PowerSheel中进行sha256校验

按下`Win`+`X`，然后按`A`打开`PowerShell`，然后运行以下命令

```powershell
Get-FileHash C:\Users\Administrator\Download\archlinux-bootstrap-2022.06.01-x86_64.tar.gz
```

- 在Linux或在macOS中进行sha256校验

Linux系统使用`Ctrl`+`Alt`+`T`打开终端，macOS用户使用`Command`+`Space`，然后搜索`Terminal`打开终端

```bash
sha256sum $HOME/Downloads/archlinux-bootstrap-2022.06.01-x86_64.tar.gz
```

<!-- tabs:end -->

<!-- 配置docsify tabs插件 -->

<!-- tabs:start -->

<!-- tab:常规方法安装 -->

### 刻录iso镜像到硬盘

- 推荐的刻录工具

| 刻录工具       | 下载链接                        | Windows    | macOS      | Linux  |
| -------------- | ------------------------------- | ---------- | ---------- | ------ |
| BalenaEtcher   | https://www.balena.io/etcher/   | 支持       | 支持       | 支持   |
| rufus          | https://rufus.ie/zh/            | 支持       | 不支持     | 不支持 |
| dd命令         | Linux自带工具                   | 不支持     | 不完美支持 | 支持   |
| 启动盘刻录工具 | 在各个Linux发行版本通常可以找到 | 可能不支持 | 可能不支持 | 支持   |

- dd命令使用教程

将U盘插入你的Linux计算机，然后备份全部数据到计算机

运行`lsblk`查看你的驱动器

假设你的U盘是`/dev/sdb`，archiso下载到了`/home/username/Downloads/archlinux-2022.05.01-x86_64.iso`，此时你需要运行以下命令完成刻录

```bash
sudo dd bs=4m if=/home/username/Downloads/archlinux-2022.05.01-x86_64.iso of=/dev/sdb
```

> **注意**
>
> - 使用dd命令时必须使用绝对路径，禁止使用相对路径（如`~/`、`$HOME/`、`../Downloads`等）
> - UltraISO为老牌的光盘创建软件且为专有软件，有一些缺陷，所以不在本文的讨论范围中
> - 无论使用哪一种刻录工具进行刻录，都必须进行`sha256`校验

### 关闭UEFI安全启动

开机以后按`F2`进入Bios（部分计算机为`delete`键）

找到安全选项（英文Bios设置选项为Security）

关闭**安全启动**`Secure Boot`（小米的笔记本如需关闭安全启动必需先设置Bios密码）

### 连接网络

#### 有线连接

最新版本的archlinux目前支持有线连接和无线连接，当你将计算机通过网络电缆或USB网络共享连接接以后，通常你就可以正常的连接网络了

如果不能正常连接，请运行以下命令`systemctl enable --now dhcpcd`

#### 无线连接

运行以下命令判断无线网卡是否正常工作

```bash
rfkill list  
```

如果无法工作，则使用以下命令假设无线网卡是`wlan0`：

```bash
rfkill unblock wlan0
```

如果你知道你的Wi-Fi SSID，你可以直接使用此命令进行连接

```bash
iwctl --passphrase mypassword station wlan0 connect mywifi
```

如果你不知道你的Wi-Fi SSID和密码，请使用以下命令

```bash
# 进入iwctl控制台
iwctl
"""以下命令均在iwctl环境下运行"""
# 列出你的所有Wi-Fi无线设备
devices list
# 此处假设你的无线设备名是wlan0，则你需要运行以下命令来扫描附近的Wi-Fi热点
station wlan0 scan
# 扫描完成以后，运行以下命令列出所有的SSID(中文名的SSID会显示乱码，如果你使用的Wi-Fi热点为中文名，请更改SSID或使用其他方式连接SSID)
station wlan0 get-networks
# 如果你要连接的Wi-Fi 的SSID是mywlan，你需要运行以下命令来连接到Wi-Fi
station wlan0 connect mywlan
# 输入密码，然后回车即可，如果没有密码可以直接回车
```

#### 验证你的网络连接

```bash
ping -c 3 https://archlinux.org/
```

> - 当你完成以上所有步骤，你就可以正常安装archlinux了

<!-- tab:使用其他Linux发行版本安装 -->

### 使用其他Linux发行版本安装

如果想要使用其他Linux发行版本安装，前提是要确保你的计算机支持UEFI启动且已经连接网络

从linux自带的软件仓库安装`arch-install-scripts`工具，然后你将会获得`arch-chroot`和`genfstab`命令

下载 archlinux-bootstrap的tar.gz文件，然后解压到任意目录

cd到`root-x86_64`目录，然后运行`arch-chroot`命令


>  **注意** 
> - 192.168.x.x是arch Linux的IPv4地址
> - 运行ssh时输入archlinux的密码, 然后可以按照archlinux的安装步骤继续安装

<!-- tab:使用ssh远程安装 -->

### 刻录iso镜像到硬盘

- 推荐的刻录工具

| 刻录工具       | 下载链接                        | Windows    | macOS      | Linux  |
| -------------- | ------------------------------- | ---------- | ---------- | ------ |
| BalenaEtcher   | https://www.balena.io/etcher/   | 支持       | 支持       | 支持   |
| rufus          | https://rufus.ie/zh/            | 支持       | 不支持     | 不支持 |
| dd命令         | Linux自带工具                   | 不支持     | 不完美支持 | 支持   |
| 启动盘刻录工具 | 在各个Linux发行版本通常可以找到 | 可能不支持 | 可能不支持 | 支持   |

- dd命令使用教程

将U盘插入你的Linux计算机，然后备份全部数据到计算机

运行`lsblk`查看你的驱动器

假设你的U盘是`/dev/sdb`，archiso下载到了`/home/username/Downloads/archlinux-2022.05.01-x86_64.iso`，此时你需要运行以下命令完成刻录

```bash
sudo dd bs=4m if=/home/username/Downloads/archlinux-2022.05.01-x86_64.iso of=/dev/sdb
```

> **注意**
>
> - 使用dd命令时必须使用绝对路径，禁止使用相对路径（如`~/`、`$HOME/`、`../Downloads`等）
> - UltraISO为老牌的光盘创建软件且为专有软件，有一些缺陷，所以不在本文的讨论范围中
> - 无论使用哪一种刻录工具进行刻录，都必须进行`sha256`校验

### 关闭UEFI安全启动

开机以后按`F2`进入Bios（部分计算机为`delete`键）

找到安全选项（英文Bios设置选项为Security）

关闭**安全启动**`Secure Boot`（小米的笔记本如需关闭安全启动必需先设置Bios密码）

> - 当你完成以上所有步骤，你就可以正常安装archlinux了

### 进入Arch Linux ISO LiveCD环境



运行`pacman -Syy openssh`安装openssh

运行`systemctl start sshd`启动ssh服务

运行`ifconfig`查看ip地址

运行`passwd`设置密码

ssh连接archlinux

```bash
# 进入root用户
sudo -i
# ssh连接arch linux
ssh root@192.168.x.x
```

### 连接网络

#### 有线连接

最新版本的archlinux目前支持有线连接和无线连接，当你将计算机通过网络电缆或USB网络共享连接接以后，通常你就可以正常的连接网络了

如果不能正常连接，请运行以下命令`systemctl enable --now dhcpcd`

#### 无线连接

运行以下命令判断无线网卡是否正常工作

```bash
rfkill list  
```

如果无法工作，则使用以下命令假设无线网卡是`wlan0`：

```bash
rfkill unblock wlan0
```

如果你知道你的Wi-Fi SSID，你可以直接使用此命令进行连接

```bash
iwctl --passphrase mypassword station wlan0 connect mywifi
```

如果你不知道你的Wi-Fi SSID和密码，请使用以下命令

```bash
# 进入iwctl控制台
iwctl
"""以下命令均在iwctl环境下运行"""
# 列出你的所有Wi-Fi无线设备
devices list
# 此处假设你的无线设备名是wlan0，则你需要运行以下命令来扫描附近的Wi-Fi热点
station wlan0 scan
# 扫描完成以后，运行以下命令列出所有的SSID(中文名的SSID会显示乱码，如果你使用的Wi-Fi热点为中文名，请更改SSID或使用其他方式连接SSID)
station wlan0 get-networks
# 如果你要连接的Wi-Fi 的SSID是mywlan，你需要运行以下命令来连接到Wi-Fi
station wlan0 connect mywlan
# 输入密码，然后回车即可，如果没有密码可以直接回车
```

#### 验证你的网络连接

```bash
ping -c 3 https://archlinux.org/
```

<!-- 在WSL上安装Arch Linux -->

<!-- tabs:end -->



## 安装准备阶段

### 配置软件源和时区

#### 关闭reflector

请运行以下命令关闭reflector服务

```bash
systemctl stop reflector
```

> **注意**:
> 无论使用哪一种安装方式，都建议你关闭reflector
> 
> 如果不关闭reflector服务，则archiso会不定期的更新软件源，可能会导致下载速度突然变慢等问题。

#### 设置时区

```bash
timedatectl set-ntp true
```

#### 将软件源切换为中国

```bash
curl -L -o /etc/pacman.d/mirrorlist "https://archlinux.org/mirrorlist/?country=CN"
```

然后编辑`/etc/pacman.d/mirrorlist`，反注释其中一个或多个链接。

### 磁盘分区

运行`lsblk`查看你的硬盘

IDE接口的硬盘一般是/dev/hdx

SATA硬盘一般是/dev/sdx

NVME固态硬盘一般是/dev/nvmexn1

假设你使用的是NVME硬盘，那么你可以运行以下命令进行分区

```bash
fdisk /dev/nvmexn1
```

选择合适的分区工具`fdisk` or `cfdisk`

建议的分区格式

| 分区     | 挂载点    | 建议大小 |
| -------- | --------- | -------- |
| efi      | /boot/efi | 300M     |
| boot     | /boot     | 500M     |
| 根分区   | /         | 剩余空间 |
| 交换分区 | NONE      | 10G      |

然后根据自己的需求选择交换分区还是交换文件

详情参考[ArchLinux磁盘分区](ArchLinuxInstallationGuide/installation/diskpart.md)

> **注意**
>
> - 交换分区分区已提前激活，分区时无需挂载交换分区


### 安装基本系统

```bash
pacstrap -i /mnt base base-devel linux-firmware vim nano  # 后面可以根据需要加入其他软件包
```

> **注意**
>
> - base-devel 不是刚需，如果你需要用aur或者是需要使用makepkg安装软件的话建议安装base-devel
> - 安装vim和nano是因为需要修改配置文件，然而arch linux不带vim和nano， 需要手动安装。
> - 此处没有安装内核，作者不在这里让你们安装内核的原因是因为想让用户可以更加自由的选择适合自己的内核。
> - 可以安装一款自己喜欢的shell，如fish、zsh，这样的话可以在chroot后直接使用自己喜欢的shell

安装后需配置fstab，这里使用`genfstab`自动配置并使用`tee`输出配置，这个命令可以省去检查fstab文件的步骤

```bash
genfstab -U /mnt | tee -a /mnt/etc/fstab
```

### chroot切换到新系统

```bash
arch-chroot /mnt /bin/bash
```

## 安装阶段

### 安装内核

|**内核名称**  |**所在仓库**   |**用途**                                             |
| ------------ | ------------- | --------------------------------------------------- |
|linux         |core           |用途广泛，常见内核                                   |
|linux-zen     |extra          |性能较好，安全性较高                                 |
|linux-lts     |core           |长期支持内核，较为稳定                               |
|linux-lily    |archlinuxcn    |archlinux社区依云自己编译的内核，对中国用户较为友好  |

如果需要安装dkms内核扩展支持，需要安装`linux-*-headers`和`dkms`两个包，其中`*`代表内核的名称，如`linux-zen`

假设你要安装普通内核，那么可以运行以下命令：`pacman -S linux linux-headers dkms`

可以选择同时安装多个内核，这样的话如果某次更新导致某个内核崩溃了，那么你可以选择其他的内核启动Arch Linux

### 配置语言

配置本地语言`vim /etc/locale.gen` 在结尾追加或反注释

```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
locale-gen`应用配置 运行`echo LANG=en_US.UTF-8 >> /etc/locale.conf
```

> **注意**
>
> - 不设置中文的原因是因为tty环境下可能会出现中文乱码，安装桌面时会安装中文字体并修改相关内容
> - 如果你使用的是另一台linux系统安装或使用装有中文的计算机上安装，这样的话你可以切换为中文

### 设置时区

本地时区配置`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

设置硬件时钟`hwclock --systohc --utc`

### 设置主机名

编辑`/etc/hostname`，写入`myhostname`

> 其中myhostname代表你的主机名，如果不配置，默认为archlinux 建议配置主机名
>
> 当你进入KDE Plasma桌面时，如果未配置主机名，可能会出现无法打开GUI应用程序的问题

### 设置hosts
编辑`/etc/hosts`
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   myhostname.localdomain myhostname
```

### 引导系统

<!-- tabs:start -->

<!-- tab:grub -->

运行`pacman -S os-prober`，如果使用archlinux单系统或者是不想用arch linux引导其他系统则可以省略此步骤

下载grub安装时所必需的文件`pacman -S dosfstools grub efibootmgr`

安装grub`grub-install --target=x86_64-efi --efi-directory=/boot/efi --recheck`

如果最后提示no error reported则说明grub安装成功

运行`grub-mkconfig -o /boot/grub/grub.cfg`更新grub

<!-- tab:systemd-boot -->

- 运行`bootctl install`

> **注意**
>
> - 请提前设置ESP分区路径，否则可能会安装失败！

- 适用systemd-boot启动archlinux

在`/boot/loader/entries`目录下添加`arch.conf`，然后将文件内容改为：

```bash
title           ArchLinux
linux           /vmlinuz-linux
initrd          /initramfs-linux.img
options         root=/dev/nvme0n1p2     rw quiet
# options         root=/dev/nvme0n1p2     rw quiet	rootflags=subvol=@ 本部分是可选的，如果你的系统是使用了btrfs分区，那么可以使用这个选项
```

> **注意**:
> - 本部分针对`linux`普通内核可以使用，如果使用`linux-zen`或`linux-lily`等内核，请自行修改配置文件
> - 如果你使用的是`linux-zen`内核，请将`vmlinuz-linux`改为`vmlinuz-linux-zen`
> - 如果你使用的是`linux-lily`或`linux-lts`内核，请按照上面的方式修改配置文件

<!-- tab:efistub -->

类似于Windows Boot Manager引导项，EFIStub由UEFI直接引导Linux内核

```bash
efibootmgr -d /dev/sda -p 1 -c -L "Arch Linux" -l /vmlinuz-linux -u "root=/dev/sda2 rw initrd=/initramfs-linux.img"
```

<!-- tabs:end -->

**提示**

- 其中的`/dev/sda`是系统所使用的磁盘
- `-p 1`中的`1`代表EFI分区的位置
- `/dev/sda2`代表root分区位置

> **注意**:
>
> - 如果你没使用grub，而且使用的不是普通的linux内核（如zen内核、lily内核或lts内核）
> - 请把`vmlinuz-linux`和`initramfs-linux.img`替换为
> - `vmlinuz-linux-zen`和`initramfs-linux-zen.img`

### 用户配置

#### 配置root用户

设置root密码`passwd`

#### 配置默认用户

添加用户`useradd -m -g users -G wheel -s /bin/bash username`

设置用户密码（可以和root相同）`passwd username`

配置默认编辑器为vim

```bash
sudo ln -sf /usr/bin/vim /usr/bin/vi
```

为用户添加sudo权限`visudo`

反注释`%wheel ALL=(ALL) ALL`

如果想在输入密码时显示星号，可以追加`Defaults env_reset,pwfeedback`

> **注意**
>
> - usename为用户名
> - 如果没有设置星号，输入密码时不显示密码是正常现象
> - 编辑/etc/sudoers时请用visudo，不要用vim直接编辑，不然可能会导致用户权限出现异常

### 网络配置

<!-- tabs:start -->

<!-- tab:WiFi -->

~~安装wifi驱动`pacman -S netctl iw wpa_supplicant dialog iwd`~~

目前仅需要安装以下软件包即可`pacman -S iwd networkmanager`

博通网卡请安装这个驱动（常用于MacBook用户或黑苹果用户）`pacman -S broadcom-wl`，如果你使用的是`linux-headers`和`dkms`，你应当安装`broadcom-wl-dkms`来代替`broadcom-wl`。

安装好以后，需要启动以下服务：

```bash
systemctl enable NetworkManager
systemctl enable iwd
```

<!-- tab:有线网络 -->

~~安装有线网卡驱动`pacman -S dhcp dhcpcd netwkrkmanager`~~

目前仅需要安装以下驱动即可`pacman -S netwotkmanager`


```bash
systemctl enable NetworkManager
```

<!-- tabs:end -->

### 退出chroot并重启计算机

```bash
# 退出chroot
exit
# 重启系统
reboot
```

> **注意**:
> 运行reboot命令重启后，请移除安装介质
