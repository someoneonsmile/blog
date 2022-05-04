# git 技巧

- [ ] `git blame` 查看每行变动
- [ ] `git init --bare` 创建空仓库, 可作为远程仓库
- [ ] `git bisect 二分查找提交历史, 查找哪一次提交破坏的单元测试
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
