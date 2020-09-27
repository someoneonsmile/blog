# linux 之间通过 nc tar 传输文件

## 发送方

```sh
tar -czvf - dir | nc -q 5 -l -p 6501
```

## 接收方

```sh
nc {{ip}} {{port}} -w 5 | pv | tar -zxf -
```
