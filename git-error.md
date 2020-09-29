# git error 解决

## 错误描述

```text
error: object file .git/objects/xx/12345 is empty
fatal: loose object xx12345 (stored in .git/objects/xx/12345 is corrupt)
```

## 解决办法

```sh
find .git/objects/ -size 0 -exec rm -f {} \;
```

## 参考

> [stackoverflow](https://stackoverflow.com/questions/4254389/git-corrupt-loose-object)
