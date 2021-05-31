# Git基本使用

- 创建新仓库

  `git init`

- 检出仓库
  - 创建一个本地仓库的克隆版本
    `git clone /path/to/repository` 
  - 如果是远端服务器上的仓库
    `git clone username@host:/path/to/repository`

- 添加与提交
  - `git add <filename>` `git add *`
  - `git commit -m "代码提交信息"`

- 推送改动

  `git push origin master`

- 仓库连接到某个远程服务器

  `git remote add origin <server>`

- 分支

  - 创建分支

    `git checkout -b feature_x`

  - 切回主分支

    `git checkout master`

  - 删除分支

    `git branch -d feature_x`

  - 推送分支

    `git push origin <branch>`

- 更新与合并

  `git pull`

  `git merge <branch>`

  `git diff <source_branch> <target_branch>`	

- 替换本地改动

    `git checkout -- <filename>`

本文转载自：git - 简易指南<http://www.bootcss.com/p/git-guide/>





