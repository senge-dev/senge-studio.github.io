# Archlinux终端配置教程

## powerlevel10k配置

运行`sudo pacman -S zsh`安装zsh 

运行`chsh -s /bin/zsh`配置zsh

如果想换回bash，运行`chsh -s /bin/bash`

### 安装oh-my-zsh

如果没安装git需要运行以下命令，否则跳过`sudo pacman -S git`

安装完成以后，终端运行以下命令(已启用CDN加速)

```bash
sh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/robbyrussell/oh-my-zsh/tools/install.sh)" 
```

> 注意：不要以sudo用户执行，否则将会把配置文件安装到`root`目录下。

### 安装PowerLevel10k主题

运行`sudo pacman -S nerd-fonts-complete`安装字体（需要从`archlinuxcn`源或`aur`安装）

运行以下命令安装powerlevel10k主题
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

配置powerlevel10k

在~/.zshrc写入`ZSH_THEME="powerlevel10k/powerlevel10k" `

安装后重新打开终端，并更改终端字体为hack

如需重新配置powerlevel10k主题，可随时运行`p10k configure`

## 安装插件

### 安装自动补全插件

```bash
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```
修改`~/.zshrc`文件
```
plugins=(
    # 其他配置
    zsh-autosuggestions
    git
)
```

### 安装语法高亮插件

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
修改`~/.zshrc`文件
```
plugins=(
    # 其他配置
    zsh-autosuggestions
    zsh-syntax-highlighting
    git
)
```
### 安装colorls插件

运行`sudo pacman -S ruby`安装Ruby Gem

运行`gem install colorls`，用gem安装colorls(确保已安装nerd-fonts字体，不然会乱码)

> - 如果安装速度慢，可以[点击这里](https://mirror.tuna.tsinghua.edu.cn/help/rubygems/)切换为清华源
`colorls`安装成功以后将下面几行写入`~/.zshrc`
```
alias ls=colorls
alias ll="colorls -l"
alias la="colorls -a"
```
> **注意**: 以上任意配置，只要涉及到修改`~/.zshrc`文件的，都要运行`source ~/.zshrc`，这样可以使配置立即生效

## 可选配置

### sudo输入密码显示星号

运行`sudo visudo`

如果无法运行，请创建软连接或修改默认的editor编辑器 

配置好以后，在配置文件中添加以下内容`Defaults env_reset,pwfeedback`

### vimplus

运行`git clone https://github.com/chxuan/vimplus.git ~/.vimplus`，从github克隆

运行`sh ~/.vimplus/install.sh`安装vimplus（可能需要五分钟左右，取决于你的网速）

运行`sudo sh ~/.vimplus/install_to_user.sh username root`安装到root用户

运行`sh ~/.vimplus/update.sh`更新vimplus

> 本软件非博主原创，原作者chxuan
> 
> 如果想查看原作者文章，请[点击此处](https://www.cnblogs.com/highway-9/p/5984285.html)
> 
> 除了vimplus，你也可以尝试一下theCW的neovim插件，配置链接如下：https://github.com/theniceboy/nvim

### GitHub Copilot插件（Neovim）

申请github copilot内测

打开[申请链接](https://copilot.github.com/)，然后过几天会收到申请邮件

为neovim安装github copilot插件，确保你已经安装`git`

终端运行
```bash
git clone https://github.com/github/copilot.vim.git ~/.config/nvim/pack/github/start/copilot.vim
```

运行`nvim`，然后运行
```
:Copilot setup
```

运行后会看到一个代码，代码格式为`ABCD-EFGH`

然后使用申请了GitHub Copilot的账户登录[验证网站](https://github.com/login/device)并绑定GitHub账户即可。

> **注意**
> - GitHub Copilot目前支持`Jetbrains全家桶`、`Visual Studsio Code`和`Neovim`
> - GitHub Copilot需要绑定GitHub账户才能使用
> - GitHub Copilot将会无视代码的开源协议，但是你可以通过GitHub设置来禁止GitHub Copilot使用你的代码
> - 目前在国内，GitHub Copilot可以正常使用，不过随着GFW的影响，后期可能会出现无法使用的情况
> - 因为开源协议的问题，请勿将使用GitHub Copilot编写的代码进行商用
> - 如果因为商用引发的一切问题和本人以及GitHub无关
> - GitHub Copilot目前开始收费
> - 如果你不想购买，你仍可以在`2022-08-22`前免费使用
> - 如果你想要购买，每个月的费用为`$10`、每年的费用为`$100`，但是GitHub Copilot只支持信用卡支付和PayPal支付，暂不支持微信支付、支付宝等国内支付方式。
