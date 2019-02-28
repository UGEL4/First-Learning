# Git笔记

## 简介
Git是目前世界上最先进的分布式版本控制系统

## 基本命令

1. 设置用户信息(Username和Email) 
> git config --global user.name "Your Name"
> git config --global user.email "email@example.com"

2. 设置Git仓库

> git init

```
创建目标目录
切换驱动器		盘符:
切换目录		cd 目录名
返回上一级目录		cd ..
回忆历史输入内容	键盘上下按键
```

3. 查看仓库状态
> git status

4. 添加文件(被Git所管理)
> git add 文件名/--all
> git commit -m "日志记录的内容"

5. 对比文件不同
> git diff 文件名

6. 查看历史记录[单行显示]
> git log [--pretty=oneline]
> git reflog

7. 版本回退
> git reset --hard HEAD^(HEAD^^/HEAD~5)
> git reset --hard commit id(commit只需写一小部分即可)

8. 撤回工作区
> git checkout -- 文件名

9. 撤销暂存区
> git reset HEAD 文件名

10. 删除版本库文件(删除后需要commit)
> git rm 文件名

11. 分支创建
> git branch 分支名

12. 切换分支
> git checkout 分支名

13. 创建并且切换分支
> git checkout -b 分支名

14. 删除分支
> git branch -d 分支名

15. 合并分支(必须在目标分支上把分支合并上来)
> git merge 被合并的分支
