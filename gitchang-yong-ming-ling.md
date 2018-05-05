### git常用命令

```
git add .  // 将本地文件添加到暂存区
git commit -m '' // 将暂存区的文件提交到本地仓库;
git pull origin <branch name> // 将远程分支代码更新到本地;
git fetch // 将远程代码更新到本地, 但是不做merge操作;
git push // 将本地仓库代码推送到远程仓库;
git checkout master // 切换到本地分支;
git checktout -b [本地分支]  upstream/[远程分支];
git remote add origin  url(远程仓库路径);
git push -u origin master // 本地代码推送带远程仓库

git remte -v // 查看仓库源

git tag -a [tag名称]  -m '解释说明';

git tag 查看本地tag;
git push upstream --tags // 本地tag推送到远程;
```
