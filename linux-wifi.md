# linux 连接 wifi

| date       | tag        |
| ---------- | ---------- |
| 2021-06-13 | linux/wifi |

## 通过 nmcli 命令行连接 wifi

`sudo nmcli device connection wifi SSID password 'password'`

## 定时巡查连接情况

`sudo crontab -u root -e`

`* * * * * connect-wifi.sh`

> connect-wifi.sh 文件如下

```sh
#!/bin/bash
status=$(nmcli connection show SSID | grep 'GENERAL.STATE' | awk '{print $2}')

if [ "$status" != "已激活" ]; then
    sudo nmcli connection up SSID
fi
```

## 两台电脑通过网线直联

在两台电脑上设置以太网为同一网段的静态 IP, 选择其中一台作为网关

## 无线路由器当作无线交换机

### 两台路由之间的连线方式

- 串联

  关闭无线路由器的 DHCP 功能, 网线从上级路由器的 LAN 口连至无线路由器的 LAN 口, 此为串联

- 级联

  开启无线路由器的 DHCP 功能, 网线从上级路由器的 LAN 口连至无线路由器的 WAN 口, 此为级联

采用串联方式, 作为无线交换机使用, 此时处于同一网段, 没有额外 nat 损耗

采用级联方式, 作为子路由器使用, 此时子路由不能与主路由网段相同, 会有 IP 冲突, 有额外 nat 损耗

> [参考](https://blog.csdn.net/weixin_42261100/article/details/112873758)
