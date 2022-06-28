# 磁盘分区

## 常用的分区命令

<!-- tabs:start -->

<!-- tab:fdisk -->


fdisk是一款命令行界面的磁盘分区工具，可以轻松的设置磁盘的可用空间

运行`lsblk`查看你的硬盘 机械硬盘一般是/dev/hdx SATA固态硬盘一般是/dev/sdx NVME固态硬盘一般是/dev/nvmexn1

```bash
fdisk /dev/nvmexn1
```

其中nvm2xn1是你的硬盘 fdisk常用命令

| 命令 | 操作                            |
| ---- | ------------------------------- |
| g    | 转化为GPT格式，常见于UEFI启动   |
| o    | 转化为dos格式，常见于LEGACY启动 |
| m    | 帮助                            |
| n    | 新建分区                        |
| d    | 删除分区                        |
| w    | 保存数据                        |
| q    | 退出但不保存                    |

<!-- tab:cfdisk -->

> - 使用cfdisk来配置，cfdisk是类似于图形界面的磁盘管理工具，功能强大，很容易使用
> - 可以设置分区的格式（默认为`Linux Filesystem`）
> - 可以灵活的调整分区大小
> - 需要修改磁盘标识为`Linux Filesystem`、`EFI`或`Linux Swap`

<!-- tabs:end -->

## 分区格式

<!-- tabs:start -->

<!-- tab:ext4/xfs/f2fs -->

建议的分区格式

| 分区     | 挂载点    | 建议大小 |
| -------- | --------- | -------- |
| efi      | /boot/efi | 300M     |
| boot     | /boot     | 500M     |
| 根分区   | /         | 50G      |
| 家目录   | /home     | 剩余空间 |
| 交换分区 | -         | 10G      |

格式化分区

```bash
# 格式化EFI分区
mkfs.vfat /dev/nvmexn1p1
# 格式化boot分区(如需使用xfs、f2fs等格式，请将ext4替换为对应的文件格式)
mkfs.ext4 /dev/nvmexn1p2
# 格式化根分区
mkfs.ext4 /dev/nvmexn1p3
# 格式化home分区
mkfs.ext4 /dev/nvmexn1p4
# 创建并激活交换分区
mkswap /dev/nvmexn1p5
swapon /dev/nvmexn1p5
```

> **注意**:
>
> - `xfs`和`f2fs`无法缩小分区大小，如果有调整分区的需求，请勿使用这两个分区
>
> - 如需使用`f2fs`，你需要安装`f2fs-tools`软件包
> - 博主喜欢将整个EFI分区挂载到`/boot`目录，但是不建议`grub`用户这样做，不然会导致`grub`无法启动

```bash
# 挂载根分区
mount /dev/nvmexn1p3 /mnt
# 在根分区下新建boot文件夹
mkdir /mnt/boot
# 挂载boot分区
mount /dev/nvmexn1p2 /mnt/boot
# 在boot目录下新建efi文件夹
mkdir /mnt/boot/efi
# 挂载efi分区
mount /dev/nvmexn1p1 /mnt/boot/efi
# 在根分区下新建home文件夹
mkdir /mnt/home
# 挂载home分区
mount /dev/nvmexn1p4 /mnt/home
```

> **提示**:
>
> - 交换分区分区已提前激活，分区时无需挂载交换分区

<!-- tab:btrfs -->

同样是用fdisk或cfdisk来进行磁盘分区，然后进行挂载

| 分区     | 挂载点    | 建议大小 |
| -------- | --------- | -------- |
| efi      | /boot/efi | 300M     |
| 根分区   | /         | 剩余空间 |
| 交换分区 | -         | 10G      |

格式化分区

```bash
# 格式化EFI分区
mkfs.vfat /dev/nvmexn1p1
# 格式化btrfs分区
mkfs.btrfs -f /dev/nvmexn1p2
# 创建并激活交换分区
mkswap /dev/nvmexn1p3
swapon /dev/nvmexn1p3
```

挂载btrfs分区

```bash
# 挂载btrfs分区
mount /dev/mydisk2 /mnt
# 新建子卷
cd /mnt
# 新建root子卷
btrfs subvol create @
# 新建home子卷
btrfs subvol create @home
# 卸载btrfs分区
cd /
umount /dev/mydisk2
# 挂载root子卷
mount /dev/mydisk2 /mnt -o subvol=@
# 挂载home子卷
mkdir -p /mnt/home
mount /dev/mydisk2 /mnt/home -o subvol=@home
# 挂载EFI分区
mkdir -p /mnt/boot/efi
mount /dev/mydisk1 /mnt/boot/efi
```

<!-- tab:luks -->

[dm-crypt - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/Dm-crypt)

<!-- tab:LVM -->

[LVM (简体中文) - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/LVM_(简体中文))

<!-- tab:zfs -->

[ZFS - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/ZFS)

<!-- tabs:end -->

## 交换文件Swapfile

- 新建一个指定大小的空白文件，这将作为计算机的交换文件
```bash
sudo dd if=/dev/zero of=/swapfile bs=1024 count=16777216  # 一个16GiB的交换文件，也可以设置`8GiB`或`32GiB`，根据计算机内存大小决定
```

- 为了安全，设置交换文件所有者为root，并设置权限为其他用户不可读不可写不可执行

```bash
sudo chown root:root /swapfile
sudo chmod 0600 /swapfile
```

- 创建并激活交换文件
 
 ```bash
 sudo mkswap /swapfile
 sudoswapon /swapfile
 ```

- 永久激活交换文件
编辑`/etc/fstab`，追加以下内容
```
/swapfile			none			  swap	  sw		  0 0
```

> **注意**:
> - 目前本文只收录了`ext4`、`xfs`、`f2fs`、`btrfs`分区的安装方式，其他安装方式正在研究中，研究完以后将会第一时间更新出来
> - 交换分区和交换文件只能选择一个，不能多选，如果你使用的是Btrfs分区且Linux内核为Linux 4.19或更老的内核，请勿使用交换文件
