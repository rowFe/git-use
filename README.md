# git-use
learn how to use git...

learn git from https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
git 官网：https://git-scm.com/book/zh/v2

====================================================
=================About Git========================== 

1. git init 初始化一个Git仓库

2. 添加文件到Git仓库 分两步
    1. git add <file> (.表示添加所有更改的文件)
    2. git commit -m <message>

3. git log 显示从最近到最远的提交日志
    git log -5 显示最近的5条提交记录
    git log --pretty=online 非详细信息 只展示版本号和提交说明

4. git status 查看状态

5. 版本回退(在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，HEAD~100往上100个版本)
    1. git reset --hard HEAD^ 回退到上一个版本
    2. git reset --hard 4bb28db.. 回退到未来的某个版本(4bb28db..指版本号)
    3. 当不记得未来某个版本的版本号时 可以使用git reflog(用来记录你的每一次命令)查看到版本信息 再使用git reset --hard 4bb28db..
    4. git log可以查看提交历史，以便确定要回退到哪个版本, git reflog可以查看命令历史，以便确定要回到未来的哪个版本

6. 工作区和暂存区 (电脑里能看到的目录就是工作区, 工作区有隐藏目录.git的是git的版本库)
    Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD
    1. git add 把文件修改添加到暂存区
    2. git commit 把暂存区的所有内容提交到当前分支

7. git diff HEAD -- readme.txt 可以查看工作区和版本库里面最新版本的区别

8. 撤销更改
    1. 未将更改添加到暂存区时想撤销更改 git checkout -- <file>
    2. 将更改添加到暂存区后想撤销更改 git reset HEAD <file> 
    3. 若已经提交更改到版本库时想撤销(未推送到远程库) 即版本回退 git reset --hard HEAD^

9.  删除文件
    1. 若不小心手动或者用rm <file>命令删除了某一个文件 使用git checkout -- <file>恢复文件
    2. git rm <file> 从版本库中删除文件

10. 添加远程库 (已有本地库，新建远程库，再关联远程库)
    1.  在github新建一个git仓库 
    2.  让本地仓库与远程仓库关联起来
        git remote add origin https://github.com/rowFe/test.git (origin是远程库的名字 Git默认叫法)
    3.  将本地内容推送到远程库 (第一次推送时需加上-u参数) git将本地master分支内容推送到远程库的master分支，并且将本地master分支与远程master分支相关联
        git push -u origin master
    4. git remote -v 查看远程库信息
    5. git remote rm origin 删除已有的一个远程库
    6. 如果想关联多个Git托管工具，比如GitHub和码云，可以分别给他们指定不同的远程库名
        1. git remote add github https://github.com/rowFe/test.git (推送命令 git push github master)
        2. git remote add gitee https://gitee.com/rowFe/test.git (推送命令 git push gitee master)

11. 从远程库克隆 git clone git@github.com:rowFe/test.git

12. 创建与合并分支
    1.  git checkout -b dev 创建dev分支并且切换到dev分支
    2.  1)相当于 
        git branch dev 创建dev分支
        git checkout dev 切换到dev分支 
    3.  git branch 查看当前分支
    4.  git merge dev 合并指定分支(dev)到当前分支(master) 直接将master分支指向dev分支的当前提交
    5.  git branch -d dev 删除dev分支

13. 解决冲突
    1.  当两个分支修改相同文件时 直接合并时(如git merge dev将dev分支当前提交合并到master分支)有可能会出现冲突, 此时需要手动处理冲突 后再git add 然后git commit 
    2.  处理冲突提交之后查看分支合并情况
        git log --graph (--pretty=oneline) (--abbrev-commit) (带参--pre.. --abb..展示合并简洁内容)

14. 分支管理策略
    1.  一般合并分支内容git merge dev 会使用Fast Forward模式 这样合并之后删除分支会丢失分支信息，是看不出曾经合并过的 
    2.  使用--no-ff禁用Fast Forward模式，这样Git会在merge时生成一个新的commit 就算后面删除了分支也能查看分支信息
        git merge --no-ff -m 'merge with no-ff' dev (因为此次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去)
    3.  master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活，团队合作一般都是自己在自己分支工作再合并

15. 当前dev分支工作未完成的情况下你需要 新建一个分支去修复一个bug时
    1.  git stash save '描述' 将当前工作存储起来
    2.  新建bug分支 修改内容后切换到master分支再git merge --no-ff -m 'sove bug' bug
    3.  继续之前的工作 切换到dev分支
        git stash list 查看所有存储list
        git stash apply stash{0} 将指定stash内容添加到工作区并且它仍然存在于stash list中
        git stash pop stash{0} 将指定stash内容添加到工作区并且将它从stash list中删除
        git stash drop stash{0} 将指定stash内容从stash list中删除

16. 当新建了一个feature分支并且在此分支更改内容并且提交了但是未合并 此时需要删除分支(未被合并的分支)时 使用 git branch -D feature 强行将分支删除 (使用-D强行删除)

17. 推送分支
    1.  git push origin master 推送本地master分支内容到远程库
    2.  抓取分支
        多人协作时，默认情况从远程库clone 默认只能看到master分支
        若这台电脑(下面称other)需要在另一个分支(比如dev)开发 则需要在本地创建和远程分支对应的分支 即 git checkout -b dev origin/dev(本地和远程分支的名称最好一致)
    3.  本地库有一个feature分支 远程库没有这个分支 若想推送分支到远程库 使用 git push --set-upstream origin feature
    4.  git pull失败并且报错提示no tracking information 原因是本地分支(dev)与远程分支(orgin/dev)的链接关系没有创建 使用git branch --set-upstream-to=origin/dev dev (git branch -u origin/dev 或者 git branch --set-upstream-to origin/dev)
    5. git branch --unset-upstream 撤销本地分支与远程分支的映射关系
    6. 本地分支可以与远程的不同命分支建立映射关系 
        如: 本地分支dev 远程分支rev (git branch -u origin/rev) 推送(push)代码时应用git push origin HEAD:rev
    7. git branch -vv 查看本地分支与远程分支的映射关系

18. Rebase
    1.  rebase操作可以把本地未push的分叉提交历史整理成直线
    2.  rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比

19. 标签
    1.  git tag <tag-name> (<版本号>) (给指定commit)打一个标签 (默认标签是打在当前分支最新的commit上的，若这个commit既在dev分支又在master分支，那么两个分支都有这个标签)
    2.  git tag 查看所有标签
    3.  git show <tag-name> 查看标签信息
    4.  git tag -a <tag-name> -m <desc> (<版本号>) (给指定版本)创建带有说明的标签，用-a指定标签名，-m指定说明文字
    5.  git tag -d <tag-name> 删除标签
    6.  git push origin (<tag-name>/-tags) 推送(指定标签/所有未推送的标签)到远程 (创建的标签默认值存储在本地)
    7.  删除一个远程标签 分两步
        1.  先删除本地 git tag -d <tag-name>
        2.  再删除远程标签 git push origin :refs/tags/<tag-name> 

20.  忽略特殊文件 (.gitignore文件) 
    1. 将不想提交到远程库的文件名/文件夹名等写入.gitignore文件 (比如node_modules、.idea、.dev、.vscode等)

21. 配置别名
    1.  git config -global alias.st status 给status起一个别名 (即使用git st可以代替git status) (-global是全局对象 设置之后对当前用户所有仓库都适用)
        其他： git config --global alias.co checkout
              git config --global alias.ci commit
              git config --global alias.br branch
              git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
    2. 每个仓库的Git配置文件都放在.git/config文件 (若不想适用别名可直接删除)
    3. 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中

22. merge rebase相关 ?????
    1.  新建dev分支(本地分支)进行开发 合并时
        1.  master分支在创建dev分支后又往前走了新的commit(即dev分支的直接祖先不是master的最新commit) (master与dev分支有分叉) 这时merge会产生一个true merge (即会多一个commit) 但是这样有一个弊端 分支会污染历史图谱(git log --graph查看历史分支多不好查看)
        所以解决方法：让dev分支的直接祖先改为master分支的最新commit (即先使用 git rebase master dev 命令将dev分支变基到master分支的最新commit上，然后切换到master分支执行git merge dev) 
        2.  master分支在创建dev分支后并未向前走新的commit(即dev分支的直接祖先就是master的最新commit) (master与dev分支无分叉) 这时merge就是fast forword (即master分支直接指向dev分支指向的最新commit)
        3.  git rebase [basebranch] [topicbranch] (basebranch为基分支，topicbranch为要变基的分支)
    2. git pull 自带merge问题
        若工作区有新的commit时 push前需要拉去远程最新的代码 最好使用git pull --rebase (防止有冲突时污染历史图)


