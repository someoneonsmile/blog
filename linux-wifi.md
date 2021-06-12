# linux 连接 wifi

|    date    |    tag     |
|    ---     |    ---     |
| 2021-06-13 | linux/wifi |

## 通过 nmcli 命令行连接 wifi

`sudo nmcli device connection wifi SSID password 'password'`

## 定时巡查连接情况

`sudo crontab -u root -e`

`* * * * * connect-wifi.sh`

> connect-wifi.sh 文件如下

```sh
#!/bin/bash
status=$(nmcli connection show SSID | grep 'GENERAL.STATE' | awk '{print $2}')                                                                            │~

if [ "$status" != "已激活" ]; then
    sudo nmcli connection up SSID
fi
```
