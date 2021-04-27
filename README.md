# 从0开始安装ArchLinux

请关注我的B站和YouTube

[B站](https://space.bilibili.com/151336873)
[YouTube](https://www.youtube.com/channel/UC7lDfWG9hE332i1zjSzzLxA)

> **注意**
> - 如果你用的是Manjaro，请参考[ManjaroWiki](https://wiki.manjaro.org)进行配置！
> - Manjaro按照该文档进行操作，可能会产生致命的问题，该问题与文档作者无关！
> - [ArchWiki](https://wiki.archlinux.org)

# ArchLinux安装教程
## 准备安装介质

### 下载arch linux

使用清华源下载arch linux
清华源[下载链接](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/)

### 刻录系统镜像

#### Windows/macOS系统

下载balenaEtcher[官方下载链接](https://www.balena.io/etcher/)
Windows也可以使用rufus[下载链接](http://rufus.ie)
不建议使用UltraISO

#### Linux系统

下载balenaEtcher[官方下载链接](https://www.balena.io/etcher/)
运行`lsblk`找到你的U盘

运行`dd bs=4m if=/dev/sdx of=/dir/archlinux.iso`刻录Linux系统
注：sdx为你的U盘
/dir/archlinux.iso为下载路径，必须为绝对路径，如：

```
/home/username/Downloads/archlinux-2020.10.01-x86_64.iso
```

一定不要使用：

```
~/Downloads/archlinux-2020.10.01-x86_64.iso
```

### 使用其他Linux发行版本安装

安装`arch-install-scripts`，大部分Linux发行版的软件仓库中都有arch安装脚本，可以直接使用`pacstrap`和`arch-chroot`命令安装。

## 进Bios安装arch linux

### 连接网络

如果电脑支持Wi-Fi，可以运行`wifi-menu`（最新版的archlinux可能移除了相关驱动）：

如果不支持Wi-Fi，可以用网线连接电脑（博主是用安卓手机USB网络共享连接电脑），然后运`dhcpcd`
运行`ping https://mirrors.tuna.tsinghua.edu.cn`检测网络连接

### 使用ssh远程安装

运行`pacman -Syy openssh`安装openssh
运行`systemctl start sshd`启动ssh服务
运行`ifconfig`查看ip地址
运行`passwd`设置密码
ssh连接archlinux

```bash
# 进入root用户
sudo -i
# ssh连接arch linux
ssh root@192.168.*.*
```

>**注意**
>192.168.*.*是arch Linux的IPv4地址
>运行ssh时输入archlinux的密码
>然后可以按照archlinux的安装步骤继续安装

### 设置时区

```bash
timedatectl set-ntp true
```

### 磁盘分区

运行`lsblk`查看你的硬盘
机械硬盘一般是/dev/hdx
SATA固态硬盘一般是/dev/sdx
NVME固态硬盘一般是/dev/nvmexn1

```bash
fdisk /dev/mydisk
```

其中mydisk是你的硬盘
fdisk常用命令

| 命令 | 操作                            |
| ---- | ------------------------------- |
| g    | 转化为GPT格式，常见于UEFI启动   |
| o    | 转化为dos格式，常见于LEGACY启动 |
| m    | 帮助                            |
| n    | 新建分区                        |
| d    | 删除分区                        |
| w    | 保存数据                        |
| q    | 退出但不保存                    |

#### 主板支持UEFI引导

| 分区     | 挂载点    | 建议大小 | 格式   |
| -------- | --------- | -------- | ------ |
| efi      | /boot/efi | 300M     | vfat   |
| boot     | /boot     | 500M     | ext4   |
| 根分区   | /         | 20G      | ext4   |
| 家目录   | /home     | 剩余空间 | xfs    |
| 交换分区 | -         | 10G      | [swap] |

#### 主板不支持UEFI引导

| 分区     | 挂载点 | 建议大小 | 格式   |
| -------- | ------ | -------- | ------ |
| 根分区   | /      | 20G      | ext4   |
| 家目录   | /home  | 剩余空间 | xfs    |
| 交换分区 | -      | 10G      | [swap] |

#### 格式化分区

`mkfs.vfat /dev/mydisk1`格式化efi分区
`mkfs.ext4 /dev/mydisk2`格式化boot分区
`mkfs.ext4 /dev/mydisk3`格式化根分区
`mkfs.xfs /dev/mydisk4`格式化home分区
`mkswap /dev/mydisk5`设置swap分区
`swapon /dev/mydisk5`激活swap分区

>**注意**
>
>- 交换分区分区在此处激活，后续无需挂载交换分区
>- 如果主板不支持UEFI引导，请忽略boot和efi分区
>- xfs不支持缩小分区，谨慎使用

#### 挂载分区

```bash
# 挂载根分区
mount /dev/mydisk3 /mnt
# 在根分区下新建boot文件夹
mkdir /mnt/boot
# 挂载boot分区
mount /dev/mydisk2 /mnt/boot
# 在boot目录下新建efi文件夹
mkdir /mnt/boot/efi
# 挂载efi分区
mount /dev/mydisk1 /mnt/boot/efi
# 在根分区下新建home文件夹
mkdir /mnt/home
# 挂载home分区
mount /dev/mydisk4 /mnt/home
```

> **注意**
>
> - 交换分区分区已提前激活，分区时无需挂载交换分区
> - 如果主板不支持UEFI引导，请忽略boot和efi分区

### 更换软件源

直接切换为中国的软件包 

```bash
curl -L -o /etc/pacman.d/mirrorlist "https://www.archlinux.org/mirrorlist/?country=CN"
```

然后编辑`/etc/pacman.d/mirrorlist`，反注释其中一个或多个链接。

### 安装基本系统

```bash
pacstrap -i /mnt base base-devel linux linux-firmware vim nano
```

>**注意**
>- base-devel 不是刚需，如果你需要用aur或者是需要使用makepkg安装软件的话建议安装base-devel
>- 安装vim和nano是因为需要修改配置文件，然而arch linux不带vim和nano， 需要手动安装。
> chroot到新系统以后新系统软件源可能会失效（如果速度变慢的话请检查一下新系统的/etc/pacman.d/mirrorlist文件）

### 配置fstab

自动配置fstab`genfstab -U /mnt >> /mnt/etc/fstab`
检查fstab`cat /mnt/etc/fstab`

> **注意**
>
>  - 无论主板是否支持UEFI都可以用这个方法安装

### 切换至新系统

```bash
arch-chroot /mnt /bin/bash
```

## 系统基本配置

### 语言设置

配置本地语言`vim /etc/locale.gen`
在结尾追加或反注释

```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

`locale-gen`应用配置
运行`echo LANG=en_US.UTF-8 >> /etc/locale.conf`

>**注意**
>
>- 不设置中文的原因是因为tty环境下可能会出现中文乱码，安装桌面时会安装中文字体并修改相关内容

### 时区设置

本地时区配置`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
设置硬件时钟`hwclock --systohc --utc`

### 配置主机名
编辑`/etc/hostname`，写入`myhostname`
>其中myhostname代表你的主机名，如果不配置，默认为archlinux

### 配置hosts
编辑`/etc/hosts`，把`myhostname`改为你自己的主机名
```bash
127.0.0.1	localhost
::1			localhost
127.0.1.1	myhostname.localdomain	myhostname
```
### 引导系统

#### Grub引导Windows或其他Linux

运行`pacman -S os-prober`，如果使用archlinux单系统或者是不想用arch linux引导其他系统则可以省略此步骤

#### 主板支持UEFI引导

下载grub安装时所必需的文件`pacman -S dosfstools grub efibootmgr`
安装grub`grub-install --target=x86_64-efi --efi-directory=/boot/efi --recheck`
如果提示no error reported则说明grub安装成功
运行`grub-mkconfig -o /boot/grub/grub.cfg`更新grub

#### 主板不支持UEFI引导

下载grub安装时所必需的文件`pacman -S grub`
安装grub`grub-install /dev/sda --recheck`
如果提示no error reported则说明grub安装成功
运行`grub-mkconfig -o /boot/grub/grub.cfg `更新grub

### 用户设置

设置主机名`vim /etc/hostname`
输入主机名，只能输入字母（如果跳过此配置，主机名默认为archlinux）
设置root密码`passwd`
添加用户`useradd -m -g users -G wheel -s /bin/bash username`
设置用户密码（可以和root相同）`passwd username`
为用户添加sudo权限`vim /etc/sudoers`
反注释`%wheel ALL=(ALL) ALL`
如果想在输入密码时显示星号，可以追加`Defaults env_reset,pwfeedback`

>**注意**
>
>- usename为用户名
>- 如果没有设置星号，输入密码时不显示密码是正常现象
>- 编辑/etc/sudoers时要用:wq!命令执行，加一个感叹号强制执行

### 安装网络驱动

#### wifi

安装wifi驱动`pacman -S netctl iw wpa_supplicant dialog`
博通网卡请安装这个驱动`pacman -S broadcom-wl`

#### 有线连接dhcp、NetworkManager

安装有线网卡驱动`pacman -S dhcp dhcpcd netwkrkmanager`
chroot环境下无法systemctl启动有线网卡

### 退出chroot并进入新系统

```bash
# 退出chroot
exit
# 卸载efi分区
umount /dev/mydisk1
# 卸载boot分区
umount /dev/mydisk2
# 卸载home分区
umount /dev/mydisk4
# 卸载根分区
umount /dev/mydisk3
# 重启电脑
reboot
```

> **注意**
>
> - 运行reboot命令后，请拔掉U盘

## 配置系统

### 网络配置

#### Wi-Fi

最新版的arch可能没有wifi-menu命令

```bash
sudo wifi-menu
```

#### dhcp

有线连接`sudo systemctl enable --now dhcpcd`
重新检查网络连接`ping https://mirrors.tuna.tsinghua.edu.cn`

#### NetworkManager
有线连接`sudo systemctl enable --now NetworkManager`
重新检查网络连接`ping https://mirrors.tuna.tsinghua.edu.cn`

### 安装驱动

#### 显卡驱动

运行`lspci | grep VGA`查看显卡型号
按照自己的显卡型号安装相应驱动

| 显卡               | 驱动名称                                               |
| ------------------ | ------------------------------------------------------ |
| 通用               | xf86-video-vesa                                        |
| Intel              | xf86-video-intel                                       |
| AMD                | xf86-video-amdgpu                                      |
| NVIDIA             | nvidia nvidia-utils cuda nvidia-settings opencl-nvidia |
| 开源MVIDIA(不推荐) | xf86-video-nouveau                                     |
| ati                | xf86-video-ati                                  |
|vmware虚拟机|xf86-video-vmware|

> **FBI Warning**
>
> - 千万不要安装nouveau，千万不要安装nouveau，千万不要安装nouveau！重要的事情说三遍。如果你不怕电脑莫名卡死，当我没说。（doge）
> - ps：博主受过nouveau的折磨

#### 触摸板驱动

`sudo pacman -S xf86-input-synaptics`笔记本专用，台式机可以忽略（如果有外置触摸板也可以安装这个驱动）

### 安装桌面

`sudo pacman -S ttf-dejavu wqy-microhei`安装中文字体
将语言改成中文`sudo vim /etc/locale.conf`
将英语注释掉，添加以下内容`LANG=zh_CN.UTF-8`
安装x窗口系统`sudo pacman -S xorg`

#### Kde桌面

简洁的kde桌面`sudo pacman -S plasma-meta plasma-nm konsole dolphin kde-connect firefox`
kde全家桶`sudo pacman -S plasma plasma-nm kde-applications`
安装完成以后`sudo systemctl enable sddm`启动sddm服务
`sudo systemctl enable NetworkManager`启动网络管理器
`sudo reboot`重启

# ArchLinux 系统配置

## 添加32位支持
编辑`/etc/pacman.conf`文件，并反注释
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
`sudo pacman -Syyu`刷新软件源
## 添加archlinuxcn源
编辑`/etc/pacman.conf`文件，并在末尾追加以下内容
```
[archlinuxcn]
SigLevel = TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
`sudo pacman -S archlinuxcn-keyring`安装archlinuxcn密钥环
`sudo pacman -Syyu`刷新软件源

**警告**
> - Manjaro不要使用archlinuxcn
> - Manjaro不要使用archlinuxcn
> - Manjaro不要使用archlinuxcn

## 安装aur
### 有archlinuxcn源
```bash
sudo pacman -S yay
```
### 无archlinuxcn源
```bash
sudo pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
sudo pacman -U yay*.tar.zst
```

> **警告**
> 
> - 非Arch系请勿使用aur（尤其是Manjaro）

## 安装optimus双显卡（需要配置multilib32位支持并安装aur）
### 安装Intel核显驱动
```bash
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```
### 安装NVIDIA独显驱动
直接安装以下显卡驱动
```bash
sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils opencl-nvidia lib32-opencl-nvidia
```
Geforce630以下到Geforce400老显卡
```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```
如果是Geforce400以下的显卡，只能安装开源驱动
```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```
### 安装OptimusManager
```bash
yay -S optimus-manager optimus-manager-qt
```
>**警告**
> - I 卡 N 卡的 modeset 选项都去掉勾选
> - 切换到英特尔核显模式前，需要选择 intel，不要选 modesettings 模式。否则会黑屏+混成不能开启
> - hybird 模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到 intel。
> - 如果你使用了混成器，调整至 OpenGl 2.0 - 平滑模式。否则切换时可能会卡 splash screen
> - 本教程仅适用于KDE桌面，其他桌面可能会有Bug
> - 暂时不支持AMD显卡，驱动作者正在开发中，预计很快就能支持

本部分教程来自[ArchLinux双显卡教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/rookie/graphic_driver)

## 安装中文输入法
新安装的arch linux系统不带中文输入法，直接安装
```bash
sudo pacman -S fcitx5-chinese-addons fcitx5-git fcitx5-gtk fcitx5-qt fcitx5-pinyin-zhwiki kcm-fcitx5
```
编辑`~/.pam_environment`
写入以下内容
```
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
```
设置开机默认启动fcitx5，编辑`~/.xprofile`
在文件中写入`fcitx5 &`
重启电脑，则可以输入中文
在语言和输入法中添加（位置可能不对，博主最近在用kde桌面）
## 安装常用软件
软件包
| 软件名      | 包名                   |
| ----------- | ---------------------- |
| 网易云音乐  | netease-cloud-music    |
|~~WPS~~          | ~~wps-office(aur)~~             |
| ~~WPS中文支持~~ | ~~wps-office-mui-zh(aur)~~      |
| 火狐        | firefox                |
| VS Code     | visual-studio-code-bin |
| 谷歌浏览器  | google-chrome          |
| OBS Studio  | obs-studio             |
| 火焰截图    | flameshot              |
| AUR         | yaourt yay（archlinuxcn） |
|anaconda|anaconda(archlinuxcn)|
|多线程下载（命令行）|axel|


> **警告**
> 
> - 非Arch系请勿使用archlinuxcn和aur（尤其是Manjaro）
> - 非Arch系请勿使用archlinuxcn和aur（尤其是Manjaro）
> - 非Arch系请勿使用archlinuxcn和aur（尤其是Manjaro）

## 安装blackarch工具包（可选）
编辑`/etc/pacman.conf`添加blackarch软件源
追加以下内容
```
[blackarch]
SigLevel = Never
Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
```
安装blackarch密钥环`sudo pacman -S blackarch-keyring`
更改blackarch软件源`sudo vim /etc/pacman.conf`
把刚才的blackarch软件源中的`Never`改为`TrustAll`
`sudo pacman -Syyu`刷新软件源
安装渗透工具`sudo pacman -S blackarch`

>警告：
> - 非Arch原生不要安装blackarch（尤其是Manjaro），不然容易滚挂！
> - Arch原生一定要按照以上步骤操作，不然会提示密钥环问题。
