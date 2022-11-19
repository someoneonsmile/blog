# linux 下搭建 Dns Server 及 DNSCrypt

## DNSCrypt

- 安装 `dnscrypt-proxy`

```sh
sudo pacman -S dnscrypt-proxy
```

- 修改配置文件

`location`: `/etc/dnscrypt-proxy/dnscrypt-proxy.toml`

```toml
server_names = ['scaleway-fr', 'cloudflare', 'Cloudflare-ipv6']
listen_addresses = ['127.0.0.1:53000', '[::1]:53000']
```

[arch wiki](https://wiki.archlinux.org/title/Dnscrypt-proxy)

- 开启服务

```
sudo systemctl enable --now dnscrypt-proxy.service
```

## dnsmasq

- 安装 `dnsmasq`

```sh
sudo pacman -S dnsmasq
```

- 修改配置文件

`location`: `/etc/dnsmasq.conf`

```
no-resolv
server=::1#53000
server=127.0.0.1#53000
# 如果不只在 Lan 公开的话，注释掉 listen-address
listen-address=::1,127.0.0.1,192.168.1.10
# 配置特定域名的 IP，跟 /etc/hosts 的区别是对子域名同样生效
address=/route.site/192.168.1.12
```

- 开启服务

```sh
sudo systemctl enable --now dnsmasq
```

## 开启防火墙

```
sudo ufw allow DNS
```
