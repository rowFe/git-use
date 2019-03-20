# git-use
learn how to use git...

====================================================
=================About Git========================== 

1. git init 初始化一个Git仓库

2. 添加文件到Git仓库 分两步
    1). git add <file> (.表示添加所有更改的文件)
    2). git commit -m <message>

3. git log 显示从最近到最远的提交日志
    git log -5 显示最近的5条提交记录
    git log --pretty=online 非详细信息 只展示版本号和提交说明

4. git status 查看状态

5. 版本回退(在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，HEAD~100往上100个版本)
    1). git reset --hard HEAD^ 回退到上一个版本
    2). git reset --hard 4bb28db.. 回退到未来的某个版本(4bb28db..指版本号)
    3). 当不记得未来某个版本的版本号时 可以使用git reflog(用来记录你的每一次命令)查看到版本信息 再使用git reset --hard 4bb28db..
    4). git log可以查看提交历史，以便确定要回退到哪个版本, git reflog可以查看命令历史，以便确定要回到未来的哪个版本

6. 工作区和暂存区 (电脑里能看到的目录就是工作区, 工作区有隐藏目录.git的是git的版本库)
    Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD
    1). git add 把文件修改添加到暂存区
    2). git commit 把暂存区的所有内容提交到当前分支

7. git diff HEAD -- readme.txt 可以查看工作区和版本库里面最新版本的区别

8. 撤销更改
    1). 未将更改添加到暂存区时想撤销更改 git checkout -- <file>
    2). 将更改添加到暂存区后想撤销更改 git reset HEAD <file> 
    3). 若已经提交更改到版本库时想撤销(未推送到远程库) 即版本回退 git reset --hard HEAD^

9. 删除文件
    1). 若不小心手动或者用rm <file>命令删除了某一个文件 使用git checkout -- <file>恢复文件
    2). git rm <file> 从版本库中删除文件

10. 添加远程库 (已有本地库，新建远程库，再关联远程库)
    1). 在github新建一个git仓库 
    2). 让本地仓库与远程仓库关联起来
        git remote add origin https://github.com/rowFe/test.git (origin是远程库的名字 Git默认叫法)
    3). 将本地内容推送到远程库 (第一次推送时需加上-u参数) git将本地master分支内容推送到远程库的master分支，并且将本地master分支与远程master分支相关联
        git push -u origin master

11. 从远程库克隆 git clone git@github.com:rowFe/test.git

12. 创建与合并分支
    1). git checkout -b dev 创建dev分支并且切换到dev分支
    2). 1)相当于 
        git branch dev 创建dev分支
        git checkout dev 切换到dev分支 
    3). git branch 查看当前分支
    4). git merge dev 合并指定分支(dev)到当前分支(master) 直接将master分支指向dev分支的当前提交
    5). git branch -d dev 删除dev分支

13. 解决冲突
    1). 当两个分支修改相同文件时 直接合并时(如git merge dev将dev分支当前提交合并到master分支)有可能会出现冲突, 此时需要手动处理冲突 后再git add 然后git commit 
    2). 处理冲突提交之后查看分支合并情况
        git log --graph (--pretty=oneline) (--abbrev-commit) (带参--pre.. --abb..展示合并简洁内容)

14. 分支管理策略
    1). 一般合并分支内容git merge dev 会使用Fast Forward模式 这样合并之后删除分支会丢失分支信息，是看不出曾经合并过的 
    2). 使用--no-ff禁用Fast Forward模式，这样Git会在merge时生成一个新的commit 就算后面删除了分支也能查看分支信息
        git merge --no-ff -m 'merge with no-ff' dev (因为此次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去)
    3). master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活，团队合作一般都是自己在自己分支工作再合并

15. 当前dev分支工作未完成的情况下你需要 新建一个分支去修复一个bug时
    1). git stash save '描述' 将当前工作存储起来
    2). 新建bug分支 修改内容后切换到master分支再git merge --no-ff -m 'sove bug' bug
    3). 继续之前的工作 切换到dev分支
        git stash list 查看所有存储list
        git stash apply stash{0} 将指定stash内容添加到工作区并且它仍然存在于stash list中
        git stash pop stash{0} 将指定stash内容添加到工作区并且将它从stash list中删除
        git stash drop stash{0} 将指定stash内容从stash list中删除

16. 当新建了一个feature分支并且在此分支更改内容并且提交了但是未合并 此时需要删除分支(未被合并的分支)时 使用 git branch -D feature 强行将分支删除 (使用-D强行删除)



