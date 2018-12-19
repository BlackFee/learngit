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

	git remote add origin https://github.com/BlackFee/learngit.git  ---origin 远程库的名字
	git push -u origin master   


















