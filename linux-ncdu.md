# linux du 的替换者 ncdu

## 用法

- 查看压缩文件

```sh
zcat export.gz | ncdu -f-
```

- 导出到文件并查看

```sh
ncdu -o- | tee export.file | ncdu -f-
```
