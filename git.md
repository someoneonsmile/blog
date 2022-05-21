# git 技巧

- [ ] `git blame` 查看每行变动
- [ ] `git init --bare` 创建空仓库, 可作为远程仓库
- [ ] `git bisect` 二分查找提交历史, 查找哪一次提交破坏的单元测试
- [ ] `git rebase` 干净的提交历史
- [ ] `git add -p` 添加文件的部分改动
- [ ] `git log --all --graph --decorate` 查看树形提交
- [ ] `git add :/` 从根目录添加全部
- [ ] `git push [origin] [commit_id]:[remote_branch]` origin: 远程仓库名, commit_id: 提交 hash, remote_branch: 远程分支名
- [ ] `git push [origin] :[remote_branch]` origin: 远程仓库名, remote_branch: 远程分支名, 删除远程分支
- [ ] `git rebase --onto=[master] [server] [client]` client 在 server 之后的提交在 master 上重放
- [ ] `git reset origin/remote_branch --hard` 重置到远程分支 HEAD
- [ ] `git reset origin/remote_branch~ --hard` 重置到 commit_id
- [ ] `git commit --amend --date $(date -R)` 重新设置 commit date
- [ ] `origin/remote_branch` 只是远程分支头结点 commit_id 的引用
- [ ] `git checkout --[outs/theirs] PATH/FILE` resolve easy/obvious conflicts


- [ ] `git rebase`: options
    - `--onto`: rebase 之后的 commit 开始位置
    - `--keep-base`: `git rebase --keep-base <upstream> <branch>` 等于 `git base --onto <upstream> <upstream>`  
        `<upstream>`: 默认为上游分支, 也可能是任务有效提交 (maybe any valid commit)  
        `<branch>`: working branch; default to `HEAD`
    - `--autosquash`: 在 `-i` 模式下, 如果 commit log 以 `squash! ...` | `fixup! ...` | `amend! ...` 开头  
        并且 `todo list` 中包含 commit 的 log 匹配 `...` 会自动修改 `todo list`  
        配合使用
        - `git commit --fixup=<commit>`
        - `git commit --fixup=amand:<commit>`
        - `git commit --fixup=reword:<commit>`
        - `git commit --squash=<commit>`

    - `--root`: rebase from the branch root
    - `-r, --rebase-merges=[=(rebase-cousins|no-rebase-cousins)]`: 保留 merge 节点

    - `--no-keep-empty, --keep-empty`: 是否保留在 rebase 之前的 empty commit
    - `--empty={drop,keep,ask}`: 是否保留 rebase 之前 not empty, rebase 之后 empty 的 commit

    - `--committer-date-is-author-date`: 用 author date, 将启用 --force-rebase
    - `--ignore-date, --reset-author-date`: 用 current time, 将启用 --force-rebase
