# 统计 tcp 状态

|    date    |     tag     |
|    ---     |     ---     |
| 2020-11-05 | awk/netstat |

## 命令

```sh
netstat -na | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```
