# Git

Git是一个开源的分布式版本控制系统，同时也是一个内容管理系统、工作管理系统等。

## Git工作流程

## Git工作区、暂存区、版本库

工作区是资源目录中的一个文件夹，保存当前项目的所有文，Git版本目录则存在于`./.git`文件夹下。

而版本库是保存用户对工作区每次修改的记录，通过版本库可以将工作区文件回退到不同时间修改之前的状态。版本库中存在着**暂存区**，是保存当前对工作区修改的具体内容，可以通过`add`指令将指定的文件添加到暂存区以便统一提交。

每当使用`add`指令时，暂存区的文件被更新，并且将此次添加到暂存区的文件内容写入到对象库的一个新对象中并且该对象的ID存储在暂存区的索引中。Git创建的对象被保存在`.git/objects`目录下，暂存区存储的就是每一个对象ID。

当执行提交`commit`指令时，暂存区的所有添加都会提交到版本库中，版本库添加一次提交记录，并且master分支会同步更新到最新的修改。当执行`reset HEAD`指令重置所有暂缓时，会销毁当前暂存区并且被当前master替换，工作区不会发生修改。当执行`rm --cached filename`时会直接从暂缓区删除指定的文件，不影响工作区文件。当执行`checkout .`或`checkout -- filename`时会使用当前暂缓区文件替换工作区文件。当执行`checkout HEAD .`或`checkout HEAD filename`时会使用当前版本库目录替换掉当前暂缓区以及工作区的文件。

## 指令集合

- 将指定文件添加到暂缓区：`git add filename1 filename2 ...`
- 将暂缓区文件提交到版本库：`git commit-m "comment"`
  - 使用`-a`可以跳过暂缓区直接提交
- 删除提交到暂缓区的文件：`git rm --cached filename`
  - 删除指定文件：`git rm filename`
  - 从暂缓区和工作区删除指定文件：`git rm -rf filename`
- 使用暂缓区文件替换工作区 全部/指定 文件：`git checkout . / git checkout -- filename`
- 使用版本库文件替换暂缓区和工作区 全部/指定 文件：`git checkout HEAD . / checkout HEAD filename`
- 设置提交用户名：`git config --global user.name "username"`
- 设置提交邮箱：`git config --global user.email "email"`
  - 当添加`--global`关键字后表明此用户为全局配置，所有项目默认使用此配置
- 使用当前目录作为一个仓库：`git init`
- 将链接目标仓库克隆到当前目录并指定项目名：`git clone [url] projectname`
- 将链接目标仓库克隆到指定目录：`git clone [url] directory`
- 查看git配置信息：`git config --list`
- 修改当前仓库配置：`git config -e configkey configvalue`
- 修改git全局配置：`git config -e --global configkey configvalue`
- 查看当前仓库状态：`git status`
- 查看暂缓区和工作区文件差异：`git diff`
  - 查看已缓存的改动：`git diff --cached`
- 清空当前添加到暂缓区的文件：`git reset HEAD`
  - 回退到上一个版本：`git reset HEAD^`
  - 回退指定文件到上一个版本：`git reset HEAD^ filename`
  - 回退到指定版本：`git reset ****`
  - 将本地仓库状态回退到远程仓库状态：`git reset -- hard origin/master`
- 查看提交历史：`git log`
  - 查看简介版本历史：`git log --oneline`
  - 查看历史记录中分支信息：`git log --graph`
  - 查看分支分叉、归并记录：`git log --reverse`
- 查看指定文件修改记录：`git blame filename`
- 显示所有远程仓库：`git remote -v`
  - 显示远程仓库信息：`git remote show [url]`
  - 添加远程版本库：`git remote add remotename [url]`
  - 提交到远程仓库：`git remote -u remotename master`
  - 删除远程仓库：`git remote rm remotename`
  - 修改远程仓库名：`git remote rename old_name new_name`
- 将远程分支合并到当前分支：`git pull remotename master`



