### git常用命令

```
git add .  // 将本地文件添加到暂存区
git commit -m '' // 将暂存区的文件提交到本地仓库;
git pull origin <branch name> // 将远程分支代码更新到本地;
git fetch // 将远程代码更新到本地, 但是不做merge操作;
git push // 将本地仓库代码推送到远程仓库;
git checkout master // 切换到本地分支;
git checktout -b [本地分支]  upstream/[远程分支]; // 创建分支
git checkout master(分支名称)  文件路径  // 获取master分支的文件;
git cherry-pick <commit id>
git remote add origin  url(远程仓库路径);
git push -u origin master // 本地代码推送带远程仓库

// 仓库
git remte -v // 查看仓库源

// 生成标签
git tag -a [tag名称]  -m '解释说明';
git tag 查看本地tag;
git push upstream --tags // 本地tag推送到远程;
git tag -d [tag名称] // 删除本地tag;
git push origin :refs/tags/[名称]　　　　//本地tag删除了，再执行该句，删除线上tag

// 暂存
git stash // 暂存;
git stash pop // 取出最新的暂存点, 会删除暂存;
git stash pop stash@{id} // 取出对应的莫个暂存点,会删除暂存;
git stash list // 查看暂存的点;
git stash drop // 删除最新点;
git stash drop stash@{id} // 删除对应的莫个暂存点;
git stash apply stash@{id} // 取出对应的暂存点, 不会删除暂存


// 创建网上能看见的分支
git checkout -b gh-pages
git push origin gh-pages
```

#### Gogs: 搭建git本地服务器;

### husky: 配置git hooks
