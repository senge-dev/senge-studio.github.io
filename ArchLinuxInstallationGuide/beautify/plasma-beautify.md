## 美化效果图

以Manjaro为例，arch同样适用

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509175252686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI1MTc4MA==,size_16,color_FFFFFF,t_70#pic_center)

## 美化过程

### 调整面板

删除自带面板，新建空面板，从左往右依次是：

> 应用程序菜单->全局菜单->面板间距->系统托盘->数字时钟
> 
> **注意**: 本配置为仿造macOS风格，如果想使用Windows或Ubuntu Unity风格，请按照自己的需求自行配置

可根据实际情况添加其他部件

### 安装主题

直接下载全局主题，依次点击

> 系统设置->全局主题->获取新的主题（如果无法下载，可以分别在Plasma样式、颜色、应用程序风格等自己选择主题进行下载，也可以下载SDDM桌面、欢迎界面、图标和鼠标指针）
> 
> 系统设置->应用程序风格->窗口装饰->标题栏按钮（自己调整窗口标题栏，可以把按键放到左边）
> 
> 系统设置->工作空间行为->桌面特效->糖果->选择三个特效->应用
> 
> 系统设置->工作空间行为->桌面特效->外观->魔灯
> 
> 系统设置->工作空间行为->桌面特效->焦点->滑出
> 
> 系统设置->工作空间行为->桌面特效->窗口管理->桌面立方
> 
> 系统设置->窗口管理->任务切换器->把微风改为翻转切换
>
> **注意**: 
> - KDE Plasma桌面在实时更新，实时使用效果可能会和文章中的不同。

### 安装Kvantum
首先运行`sudo pacman -S kvantum`，然后手动添加kvantum主题文件夹以完成安装。

### 安装Latte-Dock

安装`latte-dock`

关闭背景大小

绝对大小48

鼠标悬停放大40%

删除模拟时钟

在dock上添加部件

左侧为应用程序面板

右侧为回收站

新版的Latte Dock支持调整窗口与屏幕底部之间的间距，视觉类似于macOS Big Sur或Deepin Linux 20 或更新的版本

此时Latte-Dock可以正常使用

> **注意**:
> - 以上问题是作者在2020年使用manjaro时出现的问题，目前不知道问题是否存在。


### 安装小部件

>右键点击桌面空白部分->点击添加部件->搜索 Simple System Monitor -> 点击安装

安装后点击配置，Background Color选择Cristal，如果没有交换分区，可以自行关闭
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509175600576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI1MTc4MA==,size_16,color_FFFFFF,t_70#pic_center)

### 安装panon(可选)

直接添加部件，下载panon，然后运行命令
```bash
sudo pacman -S qt5-websockets python-docopt python-numpy python-pillow python-pyaudio python-cffi python-websockets
```

## 可选配置

### 平铺KDE（可选，比较接近i3wm）

2020年9月24日更新
[教程来自于以下链接](https://opensuse.bwsl.wang/opensuse/%E5%B9%B3%E9%93%BAKDE.html)

[点击查看](https://space.bilibili.com/268630727?from=search&seid=12844584618959748428)原作者哔哩哔哩主页

首先运行以下命令，将平铺KDE配置克隆到桌面上
```bash
git clone https://github.com/lingtjien/Grid-Tiling-Kwin.git ~/Grid-Tiling-Kwin
```

然后安装插件到Kwin脚本上
```bash
kpackagetool5 --type KWin/Script -i ~/Grid-Tiling-Kwin
```

新建必需的文件夹
```bash
mkdir ~/.local/share/kservices5
```

进行最后的配置，这样做的话可以删除刚才克隆的配置文件
```bash
cp ~/Grid-Tiling-Kwin/metadata.desktop ~/.local/share/kservices5/kwin-script-grid-tiling.desktop
```

效果图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200924193831303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTI1MTc4MA==,size_16,color_FFFFFF,t_70#pic_center)

### KDE使用i3wm窗口管理器

> **注意**:
> 
> - 如果想使用kwin脚本，请勿使用此配置，不然你的所有kwin脚本都会失效（想要切换wm为i3必须要禁用kwin）
> - 在使用i3wm的情况下，全局菜单、window-buttons等小部件会失效，桌面右键也会无反应，请谨慎考虑是否需要安装
> - latte-dock会导致窗口出现异常，影响美观，如需安装，建议禁用latte-dock

- 2022年5月12日更新

#### 安装脚本

[项目地址(Gitee)](https://gitee.com/senge-studio/archlinux_plasma_i3wm)

[项目地址(GitHub)](https://github.com/senge-studio/archlinux_plasma_i3wm)

- 安装

```bash
sh -c "$(curl -fsSL https://gitee.com/senge-studio/archlinux_plasma_i3wm/raw/master/install.sh)"
```

- 卸载

```bash
sh -c "$(curl -fsSL https://gitee.com/senge-studio/archlinux_plasma_i3wm/raw/master/uninstall.sh)"
```

> **注意**
>
> - 请勿使用root身份执行此脚本，不然会在root目录下安装此配置文件。

#### 疑难解答

- 安装完成以后我想修改壁纸怎么办

> - 新建文件夹`$HOME/.config/wallpapers`，并将壁纸放在该目录下，然后修改壁纸名为`i3wallpaper.png`
> - 手动修改配置文件第113行，将壁纸的链接粘贴到配置文件

- 如果我想切换窗口管理器为kwin但是不想删除i3配置怎么办？

> - 新版本的配置文件已经支持一键切换窗口管理器，只需运行`i3-switch`即可

- 配置文件有什么bug

> - 在i3模式下，全局菜单、window-buttons无法使用，无法添加小部件
> - 在i3模式下，所有kwin脚本都会失效（这不算Bug，切换wm为i3的目的是禁用i3wm）

#### 如何使用

- 常用快捷键`mod`键为`Meta`键、`Super`键或`Win`键，对应macOS的`command`键。

| 快捷键                | 功能                               |
| --------------------- | ---------------------------------- |
| `mod`+`shift`+`q`     | 关闭当前窗口                       |
| `mod`+`Return`        | 打开Konsole                        |
| `mod`+`d`             | 打开Krunner                        |
| `mod`+`f`             | 切换全屏模式                       |
| `mod`+`Shift`+`Space` | 切换当前窗口为浮动、平铺           |
| `mod`+`num_x`         | 切换虚拟桌面/工作空间              |
| `mod`+`Shift`+`num_x` | 将当前窗口移动到桌面/工作空间      |
| `mod`+`Shift`+`c`     | 重新加载配置文件                   |
| `mod`+`Shift`+`r`     | 重启i3                             |
| `mod`+`Shift`+`e`     | 退出登录                           |
| `mod`+`r`             | 进入窗口大小编辑模式               |
| `mod`+`Shift`+`g`     | 调整窗口间距                       |
| `Return`/`Esc`        | 退出编辑/窗口间距模式              |
| `mod`+`letter`        | 打开某个应用（请自行编辑配置文件） |

- 当你下载以后，请手动编辑`$HOME/.config/i3/config`文件，然后按照自己的喜好来修改配置文件。
