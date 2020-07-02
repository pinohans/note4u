# 常用命令清单

## 1 git

文件/文件夹包括以下几种状态

| 状态名 | 解释 |
| --- | --- |
| untracked | 新文件/文件夹 |
| tracked | 非新文件/文件夹 |
| modified | 已经tracked的文件被修改的状态 |
| unstaged | 相当于untracked，tracked，modified的集合 |
| staged | unstaged的状态被git add的状态 |

区域的几个说法

| 区域 | 解释 |
| --- | --- |
| 工作区 | 包括untracked，modified |
| 暂存区 | 包括staged |
| HEAD | 最后一次commit |

### 1.1 撤销某个版本

撤销n个版本

```bash
# git reset，撤销n个commit
$ git reset HEAD~n          # HEAD前移n个版本，并把staged的文件/文件夹全部放入工作区
$ git reset --hard HEAD~n   # HEAD前移n个版本，并把staged的文件/文件夹全部舍弃
```

清理暂存区

```bash
$ git reset HEAD        # 把staged的文件/文件夹全部放入工作区
$ git reset --hard HEAD # 把staged的文件/文件夹全部删除
```

清理工作区

```bash
# 对于untracked的文件/文件夹，全部删除
$ git clean -df
# 对于modified的文件，全部删除
$ git checkout .
```

**所以完全恢复到n个版本前指令就是**

```bash
$ git reset --hard HEAD~n && git clean -df
```


### 1.2 合并分支


有如下假设

| 名称 | 解释 |
| --- | --- |
| origin | 远程仓库 |
| master | 远程/本地master分支，协同开发主版本 |
| production | 远程production分支，是本地dev版本的上线版本 |
| dev | 本地dev分支，是远程master分支的开发版本 |

本地dev分支合并远程master分支并推送到远程production分支

```bash
$ (dev) git add -A && git commit -m '提交新版本' # 把工作区文件/文件夹提交到暂存区
$ (dev) git pull origin master:master # 更新本地master，当然也可以不更新直接和远程合并
$ (dev) git merge master 
# 解决conflict后，如果没有冲突会让你直接输入commit的消息
$ (dev) git add -A && git commit -m '提交merge版本' # 把工作区文件/文件夹提交到暂存区
$ (dev) git push origin dev:production # 推送到远程production分支
```
