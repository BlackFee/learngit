Git version 1
Git version 2
Git is free software buchong.


git init                  -----当前目录变仓库
git add file.txt          -----提交到暂存区
git commit -m 版本说明  		-----提交到工作区
git status				-------查看状态：是否有未提交的文件
git diff				------查看修改详情

git log --pretty=oneline     ------文件版本清单 加参数代表单行显示
commit id(版本号)  SHA1计算 十六进制表示

head当前版本 head^   head~100          ---------
git reset --hard head^				-------回退到上一个版本
git reset --hard commit id			-------也可以回到指定commit id对应的版本
git reflog							-------查看命令操作日志

工作区   版本库
stage（或者叫index）的暂存区
分支master   指针HEAD

git checkout -- readme.txt       ----------回到最近一次git commit或git add时的状态
工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

git reset HEAD readme.txt       -------既可以回退版本，也可以把暂存区的修改回退到工作区

rm test.txt 删除文件
git rm test.txt
git commit -m 
git checkout -- test.txt  -----------用版本库里的版本替换工作区的版本


远程仓库
	Git是分布式版本控制系统
	集中式版本控制系统SVN

	ssh-keygen -t rsa -C "youremail@example.com"  ------创建SSH Key


	先本地库，后远程库 然后关联远程库
	git remote add origin https://github.com/BlackFee/learngit.git  ---origin 远程库的名字
	git remote rm origin   ------删除远程库origin
	git remote -v     ------查看远程库信息

	git push -u origin master   -----第一次推送加-u参数
	先创建远程库，然后克隆本地仓库
	$ git clone git@github.com:michaelliao/gitskills.git  -------michaelliao/gitskills.git远程仓库地址
	要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
	Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快
分支管理
	git checkout -b dev    ------参数-b 表示创建并切换 
	类似以下两部命令
		git branch dev    ----创建分支
		git checkout dev  ----切换
	git branch  -----列出所有分支 当前分支前加*
	git merge dev   ---合并指定分支到当前分支
	git branch -d dev   ---删除指定分支  -d参数

	当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
	解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

	vi readme.txt               ------------编辑文件
	cat readme.txt                ----------查看文件内容
	git log --graph        
	git log --graph --pretty=oneline --abbrev-commit          ---------命令可以看到分支合并图
	分支管理策略
		Fast forward模式
		git merge --no-ff -m "merge with no-ff" dev         --------------参数--no-ff就可以用普通模式合并，合并后的历史有分支
	Bug分支
		git stash 	     ---储存当前工作现场
		git stash apply 	---恢复现场
		git stash drop		---删除现场
		git stash pop		---恢复并删除现场
		git stash list      ----查看
			>>>>>  stash@{0}: WIP on dev: f52c633 add merge
		git stash list		----查看stash清单
		git stash apply stash@{0} --------恢复指定的stash
	Feature分支
		开发一个新feature（功能），最好新建一个分支
		如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
	多人协作
		git remote -v  -------显示详细信息
		git remote 

		因此，多人协作的工作模式通常是这样：

		    首先，可以试图用git push origin <branch-name>推送自己的修改；

		    如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

		    如果合并有冲突，则解决冲突，并在本地提交；

		    没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

		如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

	Rebase
	    rebase操作可以把本地未push的分叉提交历史整理成直线；
	    rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
标签管理
	git tag v1.0   -------给当前分支head打标签 
	git tag v0.9 f52c633          ------指定commit id打标签

	git tag    --------查看标签清单
	git show v0.9   -----------查看标签信息
	git tag -a v0.1 -m "version 0.1 released" 1094adb    ------------创建带有说明的标签，用-a指定标签名，-m指定说明文字
	标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签


	git tag -d v0.1           ---------本地删除标签
		若已经推送到远程：先删除本地标签，然后用一下命令远程删除
			git push origin :refs/tags/v0.1             --------git push origin :refs/tags/标签名称
	git push origin v1.0      -------推送标签到远程
	git push origin --tags    ---------一次性推送所有标签

GitHub
	fork
	pull request
码云
	同一个本地文件可以关联多个远程库：需要不同的远程库名字

自定义Git
git config --global color.ui true
	忽略特殊文件
		.gitignore文件
		git add -f App.class
		git check-ignore -v App.class      -----检查.gitignore
	配置别名
		命令简写
		git config --global alias.co checkout         -------checkout 改名为co   --global是全局参数
		--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
		git config --global alias.ci commit           -------commit 改名为ci
		git config --global alias.br branch           -------branch 改名为br

		git config --global alias.unstage 'reset HEAD'
		git reset HEAD file   ====   git unstage file

		git config --global alias.last 'log -1'     --------log -1显示最后一次提交信息
		git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

		配置文件：
			.git/config  当前仓库的配置文件
			.gitconfig
		git config --global user.name "Your Name"
		git config --global user.email "email@example.com"
	搭建Git服务器
	    搭建Git服务器非常简单，通常10分钟即可完成；
	    要方便管理公钥，用Gitosis；
	    要像SVN那样变态地控制权限，用Gitolite

---------------------------------------------
Git的官方网站：http://git-scm.com
https://www.liaoxuefeng.com


