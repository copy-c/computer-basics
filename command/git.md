# git

<https://github.com/521xueweihan/git-tips>  

## 基础

工作区：实际修改的地方  
暂存区：add  
本地：commit
1.提交  
git push origin local_branch:master/other_branch  
2.删除远端  
git push origin --delete branch_name  
3.拉取  
git pull origin master:local  
4.拉取子仓  
1）更新主仓时同时拉取子仓  
git clone 主仓 --recursive  
2）异步拉取子仓  
git submodule init  
git submodule update 
5.重命名本地分支  
git branch -m <new-branch-name>  
6.暂存区和工作区的区别  
git diff --cached  
7.放弃工作区的修改  
git checkout <file name>
git checkout . // 所有  
  
## 组合

1.放弃本地所有修改，回到远程仓库状态  
git fetch --all && git reset --hard origin/master  
