# 从0开始安装ArchLinux

## 注意事项

本程序仅为ArchLinux原生版本准备，ArchLinux的衍生版本不在讨论的范围内

本程序的部分教程来自[ArchWiki](https://wiki.archlinux.org)

本博客仅为支持**统一可扩展固件接口**`UEFI`的计算机提供支持，使用**传统引导方式**`Legacy`的计算机不在本博客的讨论范围内

本博客只支持x86_64架构的计算机，采用arm架构的计算机不在讨论范围内

## 关于Manjaro等衍生版本的问题

如果你使用的是Manjaro，请勿参考此配置，不然可能会导致系统不稳定甚至是发生依赖破损(滚挂)的问题

Manjaro用户请移步[Manjaro Wiki](https://wiki.manjaro.org/index.php/Main_Page)

Manjaro如果想安装类似于`QQ`、`微信`、`钉钉`、`网易云音乐`等专有软件，请勿使用archlinuxcn源，不过Manjaro用户可以使用manjarocn源

本教程只会教Manjaro用户如何配置cn源，不会为Manjaro用户提供额外的技术支持

### Manjaro用户配置cn源

编辑`/etc/pacman.d/manjarocn`
```
Server = https://repo.manjarocn.org/stable/x86_64
```

编辑`/etc/pacman.conf`
```
[manjarocn]
SigLevel = TrustAll
Include = /etc/pacman.d/manjarocn
```

导入PGP-Key
```bash
sudo pacman-key --recv-keys 974B3711CFB9BF2D && sudo pacman-key --lsign-key 974B3711CFB9BF2D
```
