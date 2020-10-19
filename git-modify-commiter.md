# git 中修改已提交 commit 的用户名邮箱信息

|    date    |      tag      |
|    ---     |      ---      |
| 2020/10/19 | git/committer |

## 问题描述

github 中用户名邮箱都对应才会显示提交者为 github 账号

## 解决办法

```sh
#!/bin/sh

git filter-branch --env-filter '

an="$GIT_AUTHOR_NAME"
am="$GIT_AUTHOR_EMAIL"
cn="$GIT_COMMITTER_NAME"
cm="$GIT_COMMITTER_EMAIL"

if [ "$GIT_COMMITTER_EMAIL" = "[Your Old Email]" ]
then
    cn="[Your New Author Name]"
    cm="[Your New Email]"
fi
if [ "$GIT_AUTHOR_EMAIL" = "[Your Old Email]" ]
then
    an="[Your New Author Name]"
    am="[Your New Email]"
fi

export GIT_AUTHOR_NAME="$an"
export GIT_AUTHOR_EMAIL="$am"
export GIT_COMMITTER_NAME="$cn"
export GIT_COMMITTER_EMAIL="$cm"
'
```
