# ArchLinux 配置

## 配置系统

### 安装驱动

#### 显卡驱动

运行`lspci | grep VGA`查看显卡型号

按照自己的显卡型号安装相应驱动

##### NVIDIA显卡

<!-- tabs:start -->

<!-- tab:GeForce920以后 -->

新款显卡，直接安装最新的软件包即可

```bash
sudo pacman -S nvidia nvidia-settings nvidia-utils lib32-nvidia-utils opencl-nvidia lib32-opencl-nvidia
```

较老的GeForce显卡，要从AUR安装旧版本的NVIDIA驱动

<!-- tab:GeForce 630到GeForce 920 -->

```bash
yay -S nvidia-470xx-dkms nvidia-settings lib32-nvidia-470xx-utils linux-headers
```

<!-- tab:GeForce 400到GeForce 630 -->

旧版的GeForce显卡，需要从AUR安装旧版本的NVIDIA驱动

```bash
yay -S nvidia-390xx-dkms nvidia-390xx-utils lib32-nvidia-390xx-utils
```

<!-- tab:GeForce 400以下 -->

远古显卡，AUR没有适配的专有驱动，只能使用开源的NVIDIA驱动nouveau

```bash
sudo pacman -S mesa lib32-mesa xf86-video-nouveau
```

<!-- tabs:end -->

> **FBI Warning**
>
> - 如果你使用的是新显卡（如GeForce RTX系列），那么千万不要安装nouveau，否则极有可能导致显卡无法正常工作从而导致电脑卡顿、死机等问题。


##### AMD显卡

<!-- tabs:start -->

博主没有AMD显卡，这个配置是根据当前的主流AMD显卡型号来配置的，可能不适用于旧版的AMD显卡

```bash
sudo pacman -S mesa lib32-mesa xf86-video-amdgpu vulkan-radeon lib32-vulkan-radeon libva-mesa-driver lib32-libva-mesa-driver mesa-vdpau lib32-mesa-vdpau
```

<!-- tabs:end -->

#### 触摸板驱动

`sudo pacman -S xf86-input-libinput`笔记本专用，台式机可以忽略（如果有外置触摸板也可以安装这个驱动）

### 安装桌面

安装前要先配置中文字体并将系统界面改为中文

`sudo pacman -S noto-fonts-cjk`安装中文字体

将语言改成中文`sudo vim /etc/locale.conf`

将英语注释掉，添加以下内容`LANG=zh_CN.UTF-8`

安装x窗口系统`sudo pacman -S xorg`

<!-- tabs:start -->

<!-- tab:KDE Plasma -->

简洁的kde桌面：安装`plasma`、`plasma-nm`、`konsole`、`dolphin`、`kde-connect`、`firefox` 

完整的kde桌面：安装了上述软件包后，还要安装`kde-applications`

安装完成以后启动`sddm`服务，然后运行`reboot`重启

中文输入法（fcitx5）

```bash
sudo pacman -S fcitx5-chinese-addons fcitx5-im fcitx5 fcitx5-pinyin-zhwiki
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


<!-- tab:Gnome -->

基本的Gnome桌面：安装`gnome`、`gnome-extra`

完整的Gnome桌面：安装了上述软件包以后，再安装`gnome-tweak-tool`、`gnome-shell-extensions`、`gnome-shell-extension-gsconnect`、`gnome-shell-extension-dash-to-dock`

安装后启动`gdm`服务，然后重启计算机

安装ibus中文输入法

```bash
sudo pacman -S ibus ibus-libpinyin
```

安装完成后打开设置界面，通过“键盘”中的“输入源”添加。

在你添加至少两个输入源后，GNOME 会在托盘中显示输入选择图标。

如果如此操作之后你没有成功，很可能你没有完成 locale-gen。

默认切换输入法的快捷键是 `Super`+`Space`

<!-- tabs:end -->

### ArchLinux 安装后配置

#### 添加32位支持

编辑`/etc/pacman.conf`文件，并反注释

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

`sudo pacman -Syu`刷新软件源

#### 添加archlinuxcn源

编辑`/etc/pacman.conf`文件，并在末尾追加以下内容

```
[archlinuxcn]
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

<!-- tabs:start -->


<!-- tab:Git -->

如果你不想安装archlinuxcn源，你可以直接使用`git`克隆并使用`makepkg`安装

```bash
sudo pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

<!-- tab:ArchLinuxCN -->

如果有archlinuxcn源，可以直接从archlinuxcn源中安装

```bash
sudo pacman -S yay
```

<!-- tabs:end -->

> **警告**
>
> - 非ArchLinux请勿使用aur（尤其是Manjaro）
> - EndeavourOS、Garuda Linux等和Arch Linux用同一仓库的衍生版本可以使用cn源和aur

### 安装OptimusManager双显卡支持

安装前请确保你的笔记本电脑为双显卡：如：Intel/AMD核显+NVIDIA独显

如果你使用的是单显卡的轻薄本（如Intel Evo认证笔记本），你无需安装Optimus Manager，即使你要使用雷电4外接显卡

台式机用户也没必要安装Optimus Manager，因为Optimus Manager的主要作用是提升笔记本等便携设备的电池续航

```bash
yay -S optimus-manager optimus-manager-qt
```

> **警告**
> - I 卡 N 卡的 modeset 选项都去掉勾选
> - 切换到英特尔核显模式前，需要选择 intel，不要选 modesettings 模式。否则会黑屏+混成不能开启
> - hybird 模式中添加的三个环境变量，在切换到其他模式之前一定要去掉，否则会黑屏，切换不到 intel。
> - 如果你使用了混成器，调整至 OpenGl 2.0 - 平滑模式。否则切换时可能会卡 splash screen
> - 本教程仅适用于KDE桌面，其他桌面可能会有Bug
> - 暂时不支持AMD显卡，驱动作者正在开发中，预计很快就能支持

本部分教程来自[ArchLinux双显卡教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/rookie/graphic_driver)

### 安装常用软件

软件包

| 软件名               | 包名                                   |
| -------------------- | -------------------------------------- |
| 网易云音乐           | yesplaymusic<sup>aur</sup>             |
| WPS              | wps-office<sup>aur</sup>                   |
| WPS中文支持      | wps-office-mui-zh<sup>aur</sup>            |
| 火狐                 | firefox                                |
| VS Code              | visual-studio-code-bin<sup>aur</sup>   |
| 谷歌浏览器           | google-chrome<sup>aur</sup>            |
| OBS Studio           | obs-studio                             |
| 火焰截图             | flameshot                              |
| AUR                  | yaourt yay<sup>archlinuxcn</sup>       |
| anaconda             | anaconda<sup>archlinuxcn</sup>         |
| 多线程下载（命令行） | axel                                   |
| QQ                   | icalingua<sup>archlinuxcn</sup>        |
| 微信                 | wechat-uos<sup>aur</sup>               |
| Telegram             | telegram-desktop                       |

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

把刚才的blackarch软件源中的`SigLevel = Never`这一行删除

`sudo pacman -Syu`刷新软件源

安装渗透工具`sudo pacman -S blackarch`

> 警告：
>
> - 非ArchLinux不要安装blackarch（尤其是Manjaro），不然容易滚挂！
> - Arch原生一定要按照以上步骤操作，不然会提示密钥环问题。
