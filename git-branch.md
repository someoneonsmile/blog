# git branch

|    date    |    tag     |
|    ---     |    ---     |
| 2020-12-27 | git branch |

## 创建

`git checkout -b new_branch_name`

从 `branch_name` 分支, 创建一个 `new_branch_name` 分支

`git checkout -b new_branch_name branch_name`

## 推送

`git push origin branch_name:remote_branch_name`

`git push --set-upstream origin remote_branch_name`

`git push -u origin remote_branch_name`

## 删除本地分支

`git branch -d branch_name`

`git branch --delete branch_name`

## 删除远程分支

`git push origin --delete remote_branch_name`

`git push origin -d remote_branch_name`

推送一个空分支到远程, 来删除远程分支

`git push origin :remote_branch_name`

## 设置追踪分支

`git branch --set-upstream-to=origin/remote_branch_name`

`git branch -u origin/remote_branch_name`

## 删除追踪分支

`git branch --delete --remotes origin/remote_branch_name`

`git branch -d -r origin/remote_branch_name`

`git branch --unset-upstream`

## 分支重命名

`git branch -m branch_name new_branch_name`

## 查看分支情况

`git branch -vv`

## 分支查找

`git branch --merged`
`git branch --no-merged`
`git branch --contains <commit>`
`git branch --no-contains <commit>`

