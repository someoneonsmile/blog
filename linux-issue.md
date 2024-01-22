# 我遇到的 Linux 问题

| date       | tag         |
| ---------- | ----------- |
| 2024-01-22 | linux/issue |

## ssh 配置完成私钥登录后，仍需要输入密码

`～/.ssh/authorized_keys` 权限为 `777` 导致, `chmod -R 700 ~/.ssh` 解决
