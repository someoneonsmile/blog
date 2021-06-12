# linux 局域网共享文件

|    data    |    tag     |
|    ---     |    ---     |
| 2021-06-12 | file-share |

## 安装 samba

`pacman -S samba`

## 配置

```
[global]
# default
workgroup = WORKGROUP
netbios name = Manjaro
security = user
map to guest = bad user
guest account = nobody

load printers = no
printing = bsd
printcap name = /dev/null
disable spoolss = yes
show add printer wizard = no

[share]
comment = share
# /home/ws/share 没有权限也访问不了
path = /home/ws/share/movie
public = yes
browseable = yes
read only = yes
guest ok = yes
```

## 启动

`systemctl enable smb`
