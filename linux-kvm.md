# linux 安装 kvm

## 检查 kVM 支持

### 检查硬件支持

```sh
LC_ALL=C lscpu | grep Virtualization
```

或者

```sh
grep -E --color=auto 'vmx|svm|0xc0f' /proc/cpuinfo
```

如果运行任一命令后未显示任何内容，则表示您的处理器不支持硬件虚拟化，您将无法使用 KVM

### 检查内核支持

- 检查内核中是否有必要的模块 `kvm` 以及 `kvm_amd` 或 `kvm_intel`

```sh
zgrep CONFIG_KVM= /proc/config.gz
```

该模块仅当设置为 y 或 m 时才可用

- 检查内核模块自动加载

```sh
lsmod | grep kvm
```

如果命令没有返回任何内容，则需要手动加载模块

## 设置内核模块开机自动加载

```sh
sudo tee /etc/modules-load.d/kvm.conf <<'EOF'
kvm
kvm_amd   # 如果你是 Intel 则改成 kvm_intel
EOF
```

## 安装软件包

```sh
sudo pacman -S qemu libvirt virt-manager edk2-ovmf virt-install
```

| 包名             | 作用                                      |
| ---------------- | ----------------------------------------- |
| **qemu**         | 提供虚拟机运行核心（CPU/内存/磁盘虚拟化） |
| **libvirt**      | 虚拟化后台管理服务，统一管理 QEMU/KVM 等  |
| **edk2-ovmf**    | 提供 UEFI 固件支持（虚拟机 BIOS 替代品）  |
| **virt-manager** | 图形界面管理虚拟机（可选但实用）          |
| **virt-install** | 纯命令行创建虚拟机的工具（可选但实用）    |

## 启用 libvirtd 服务

```sh
sudo systemctl enable --now libvirtd
```

验证运行

```sh
sudo virsh list --all
```

应输出空表格但无错误

## （可选）非 root 使用

```sh
sudo usermod -aG libvirt $(whoami)
newgrp libvirt
```

## 💡 小贴士

如果你使用 libvirt / KVM NAT 网络，`UFW` 可能会阻止虚拟机访问外网
若遇到这种情况，可以运行：

```sh
sudo ufw allow in on virbr0
sudo ufw allow out on virbr0
```

或使用

```sh
sudo ufw route allow
```

规则放行 NAT 转发

