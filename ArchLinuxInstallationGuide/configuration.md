# ArchLinux 配置

## 配置系统

### 网络配置

#### Wi-Fi

首先运行`sudo systemctl enable --now iwd`来启动wifi服务

如果你的电脑支持Wi-Fi，且处于无线局域网的覆盖范围内，这样的话你可以尝试使用无线连接进行安装

安装时要运行`iwctl`，进入iwd控制台

然后输入`device list`列出网络设备的名称

如果你的网络设备的名称是`wlan0`，你可以运行`station wlan0 scan`来扫描附近的Wi-Fi热点

扫描完成后，运行`station wlan0 get-networks`

如果你要连接的Wi-Fi 的SSID是`mywlan`，你需要运行`station wlan0 connect mywlan`，然后你需要输入密码以连接网络

如果你知道网络设备的名称、Wi-Fi的SSID和Wi-Fi密码，你可以在不运行`iwctl`的模式下直接运行以下命令来连接Wi-Fi（此处我们假设Wi-Fi SSID是`mywifi`，密码是`mypassword`，网络设备名是`wlan0`）

```bash
iwctl --passphrase mypassword station wlan0 connect mywifi 
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
| ati                | xf86-video-ati                                         |
| vmware虚拟机       | xf86-video-vmware                                      |

> **FBI Warning**
>
> - 千万不要安装nouveau，千万不要安装nouveau，千万不要安装nouveau！重要的事情说三遍。如果你不怕电脑莫名卡死，当我没说。（doge）
> - ps：博主受过nouveau的折磨

#### 触摸板驱动

`sudo pacman -S xf86-input-libinput`笔记本专用，台式机可以忽略（如果有外置触摸板也可以安装这个驱动）

### 安装桌面

`sudo pacman -S noto-fonts-cjk`安装中文字体

将语言改成中文`sudo vim /etc/locale.conf`

将英语注释掉，添加以下内容`LANG=zh_CN.UTF-8`

安装x窗口系统`sudo pacman -S xorg`

#### Kde桌面

简洁的kde桌面`sudo pacman -S plasma-meta plasma-nm konsole dolphin kde-connect firefox` 

kde全家桶`sudo pacman -S plasma plasma-nm kde-applications`

安装完成以后运行`sudo systemctl enable sddm`启动sddm服务

运行`sudo systemctl enable NetworkManager`启动网络管理器

最后运行`reboot`重启

### ArchLinux 安装后配置

#### 添加32位支持

编辑`/etc/pacman.conf`文件，并反注释

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

`sudo pacman -Syyu`刷新软件源

#### 添加archlinuxcn源

编辑`/etc/pacman.conf`文件，并在末尾追加以下内容

```
[archlinuxcn]
SigLevel = TrustAll
Include = /etc/pacman.d/archlinuxcn
```

编辑`/etc/pacman.d/archlinuxcn`，将以下内容写入文件
```
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

运行`sudo pacman -S archlinuxcn-keyring`安装archlinuxcn密钥环

添加后，运行`sudo pacman -Syu`刷新软件源

>**警告**
>
> - Manjaro不要使用archlinuxcn
> - Manjaro不要使用archlinuxcn
> - Manjaro不要使用archlinuxcn

### 安装aur

```bash
sudo pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
sudo pacman -U yay*.tar.zst
```

如果有archlinuxcn源，可以直接从archlinuxcn源中安装

```bash
sudo pacman -S yay
```

> **警告**
>
> - 非ArchLinux请勿使用aur（尤其是Manjaro）
> - EndeavourOS、Garuda Linux可以使用cn源和aur

### 安装optimus双显卡（需要配置multilib32位支持并安装aur）

#### 安装Intel核显驱动

```bash
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

#### 安装NVIDIA独显驱动

- 较新型号的显卡

```bash
sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils opencl-nvidia lib32-opencl-nvidia
```

- Geforce630以下到Geforce400老显卡

```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```

- 如果是Geforce400以下的显卡，只能安装开源驱动

```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```

#### 安装OptimusManager

```bash
yay -S optimus-manager optimus-manager-qt
```

> **警告**
>
> - I 卡 N 卡的 modeset 选项都去掉勾选
> - 切换到英特尔核显模式前，需要选择 intel，不要选 modesettings 模式。否则会黑屏+混成不能开启
> - hybird 模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到 intel。
> - 如果你使用了混成器，调整至 OpenGl 2.0 - 平滑模式。否则切换时可能会卡 splash screen
> - 本教程仅适用于KDE桌面，其他桌面可能会有Bug
> - 暂时不支持AMD显卡，驱动作者正在开发中，预计很快就能支持

本部分教程来自[ArchLinux双显卡教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/rookie/graphic_driver)

### 安装中文输入法

新安装的ArchLinux系统不带中文输入法，直接安装

```bash
sudo pacman -S fcitx5-chinese-addons fcitx5-im fcitx5 fcitx5-pinyin-zhwiki kcm-fcitx5
```

编辑`/etc/environment`并写入以下内容

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
```

然后重启电脑，则可以输入中文

### 安装常用软件

软件包

| 软件名               | 包名                      |
| -------------------- | ------------------------- |
| 网易云音乐           | yesplaymusic(aur)         |
| WPS              | wps-office(aur)           |
| WPS中文支持      | wps-office-mui-zh(aur)    |
| 火狐                 | firefox                   |
| VS Code              | visual-studio-code-bin    |
| 谷歌浏览器           | google-chrome             |
| OBS Studio           | obs-studio                |
| 火焰截图             | flameshot                 |
| AUR                  | yaourt yay（archlinuxcn） |
| anaconda             | anaconda(archlinuxcn)     |
| 多线程下载（命令行） | axel                      |
| QQ                   | icalingua                 |
| 微信                 | wechat-uos                |
| Telegram             | telegram-desktop          |

> **警告**
>
> - 非ArchLinux请勿使用archlinuxcn和aur（尤其是Manjaro）
> - 非ArchLinux请勿使用archlinuxcn和aur（尤其是Manjaro）
> - 非ArchLinux请勿使用archlinuxcn和aur（尤其是Manjaro）

### 安装blackarch工具包（可选）

编辑`/etc/pacman.conf`添加blackarch软件源并追加以下内容

```
[blackarch]
SigLevel = Never
Include = /etc/pacman.d/blackarch
```

编辑/etc/pacman.d/blackarch，将以下内容写入
```
Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
```

安装blackarch密钥环`sudo pacman -S blackarch-keyring`

更改blackarch软件源`sudo vim /etc/pacman.conf`

把刚才的blackarch软件源中的`Never`改为`TrustAll`

`sudo pacman -Syyu`刷新软件源

安装渗透工具`sudo pacman -S blackarch`

> 警告：
>
> - 非ArchLinux不要安装blackarch（尤其是Manjaro），不然容易滚挂！
> - Arch原生一定要按照以上步骤操作，不然会提示密钥环问题。
