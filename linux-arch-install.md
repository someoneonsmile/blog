# 安装 arch 过程记录

| date       | tag          |
| ---------- | ------------ |
| 2021-07-10 | arch install |

## 分区

> 不分交换分区改用交换文件（可控大小）

> 如果用 efi 模式，分 efi 分区 500M 左右，gpt 模式

```sh
# 查看磁盘
fdisk -l
# 磁盘分区
fdisk /dev/sdx
# 磁盘格式化
mkfs.ext4 /dev/sdxx
# 挂载磁盘
mount /dev/sdxx /mnt
```

### fdisk 使用

```sh
# 查看磁盘
fdisk -l

# 分区
fdisk /dev/sdx

# fdisk 命令
m 使用帮助
o 创建空的 mbr 分区表
g 创建空的 gpt 分区表
n 创建新的分区
    +500M
    +20G
w 写入磁盘并退出
```

## pacstrap 安装到 /mnt

```sh
pacstrap /mnt base base-devel linux linux-firmware vim openssh
```

## 写分区表

```sh
genfatab -U /mnt > /mnt/etc/fstab
```

> 挂载 tmpfs

```sh
cat <<EOF >> /mnt/etc/fstab
tmpfs /tmp      tmpfs defaults,noatime,mode=1777 0 0
tmpfs /var/log  tmpfs defaults,noatime,mode=1777 0 0
tmpfs /var/tmp  tmpfs defaults,noatime,mode=1777 0 0
EOF
```

## 进入新系统

```sh
arch-chroot /mnt
# timezone
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# locale
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen
# lang
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
# keymap
echo "KEYMAP=us" >> /etc/vconsole.conf
# hostname
echo "myhostname" > /etc/hostname
cat <<EOF >> /etc/hosts
127.0.0.1 localhost
::1       localhost
127.0.1.1 myhostname.localdomain myhostname
EOF

# 安装软件
pacman -S networkmanager wpa_supplicant wireless_tools netctl dialog
systemctl enable sshd
systemctl enable NetworkManager
```

## 安装 grub

- for mbr not efi

```sh
pacman -S grub dosfstools os-prober mtools
grub-install --targe=i386-pc --recheck /dev/sdx # not sdxx
grub-mkconfig -o /boot/grub/grub.cfg
```

- for gpt efi

```sh
pacman -S grub efibootmgr dosfstools os-prober mtools
mkdir /boot/efi
mount /dev/sdxx /boot/efi
grub-install --targe=x86_64-efi --bootloader-id=grub_uefi --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

## 配置开机启动动画

### 安装 `plymouth`

```sh
pacman -S plymouth
```

### plymouth 开机启动

```sh
systemctl enable plymouth
```

### 配置内核参数及 grub

- 添加 init 启动 hook

`/etc/mkinitcpio.conf` `HOOKS` 参数列表最后添加 `plymouth`

```sh
mkinitcpio -p linux
```

- 配置内核参数显示启动动画

编辑 `/etc/default/grub` 添加如下参数, 让启动过程安静, 并显示启动动画

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet splash"
```

编译 grub 配置

```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

### 配置 plymouth

```sh
# 列出主题列表
plymouth-set-default-theme -l
# 设置主题为 <themename>
plymouth-set-default-theme -R <themename>
```

或修改 `/etc/plymouth/plymouthd.conf`

## 重启并进入系统

### 更换国内镜像

```sh
cat <<EOF /etc/pacman.d/mirrorlist
## Country : China
Server = https://mirrors.huaweicloud.com/archlinux/$repo/$arch

## Country : China
Server = https://mirrors.aliyun.com/archlinux/$repo/$arch

## Country : China
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/$arch

## Country : China
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/$arch

## Country : China
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux/$repo/$arch

## Country : China
Server = https://mirrors.pku.edu.cn/archlinux/$repo/$arch
EOF

cat <<EOF >> /etc/pacman.conf
[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
EOF
```

```sh
pacman -Sy
pacman -S archlinuxcn-keyring
pacman -S yay
```

## 桌面环境

```sh
# font
sudo pacman -S nerd-fonts-complete

sudo pacman -S neofetch
# 可视
sudo pacman -S xorg xorg-xinit
# 窗口管理器
sudo pacman -S wspwm sxhkd
# 合成器
sudo pacman -S picom
# bar
sudo pacman -S polybar
# soft search
sudo pacman -S rofi
```

## 取消笔记本合盖挂起

```sh
echo 'HandleLidSwitch=ignore' | sudo tee -a /etc/systemd/logind.conf
```

## 开机桌面环境 CLI 切换

```sh
# CLI
sudo systemctl set-default multi-user.target

# 桌面
sudo systemctl set-default graphical.target
```

## 虚拟机安装增强工具

```sh
sudo pacman -S virtualbox-guest-utils
sudo systemctl enable -now vboxservice.service

# 手动执行或加入到自启动
VBoxClient-all
```
