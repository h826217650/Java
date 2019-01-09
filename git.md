
# git 基本概念  
* 版本库(repository):类似一个目录，所有文件被git管理，可以修改，删除。  
* 版本号(commit id):SHA1计算出来的用十六进制标记git提交时间线  
* 工作区 ：当前电脑看到的目录  
* 暂存区(stage):执行``` git add ```就是把文件添加到暂存区  ,执行完``` git commit ```后所有修改提交到分支，暂存区变干净
* 远程仓库：存放版本库。本地Git仓库通过SSH与远程仓库传输信息


# 本地操作  
## 1 创建本地仓库  
1. 创建一个空目录
2. 通过git init把目录变为本地仓库```git init ```        
**执行上述命令后会生成一个.git目录，用来进行git版本跟踪管理。此文件不要手动修改**
3. 查看.git目录 ``` ls -ah ```  


## 2 添加文件到仓库  
1. 一个文件放入Git仓库只需要两步   
``` 1. git add hxy.txt```  
``` 2. git commit -m 'description'```    
2. 如做了多处提交，可使用``` git log ```查看历史提交记录  
3. 如修改了文件 ，可使用``` git status```查看当前状态
4. 如查看具体修改了什么内容，可使用``` git diff hxy.txt```  
5. 使用``` git diff HEAD -- hxy.txt```可查看工作区与版本库最新版本的区别  

## 3 版本回退


* 当前版本：HEAD
* 上一个版本：HEAD^
* 上上上个版本：HEAD+3 
 
git内部有个指向当前版本的HEAD指针，实现版本的回退需执行  
1. 回退到上一个版本``` git reset --hard HEAD^  ```  
2. 回退到任意一个版本  
```1. 查看提交历史记录，确定回退版本的commitID：git log```
```2. 执行：git reset --hard commitID```  
3. 重返到回退前的新版本   
```1. 查看所有的commitID ： git reflog```  
```2.找到想要穿越未来的commitID：git reset --har commitid```

## 4 撤销修改  
* 如果想撤销对某个文件的修改，可以使用以下命令实现  
 1. 修改内容后未add到暂存区时，可以使用```git checkout --file ```  
 2. 修改内容后add到暂存区后，可以使用 ``` git reset HEAD <file>```  
 3. 修改内容后commit到版本库后，可以使用“版本回退”相关操作实现  
 
* 如果删除了本地文件，可以使用以下命令删除版本库或恢复本地  
 代码commit到版本库后，删除了本地某个文件。
 
 1. 使用``` git status``` 查看工作区域版本库的区别  
 2. 如果确认要删除某个文件，可执行以下命令实现版本库文件的删除  
 ``` git rm```   
 ``` git commit```  
   
 3. 如果是误操作，想恢复工作区被删除的文件，可执行  
 ```git checkout -- file ```   
 ⚠️ ``` git checkout``` 用版本库的版本替换工作区的版本  
 
 ## 暂存区  
 git 本地仓库提供了暂存区，当手头工作没有完成时 可以先把工作现场保存下来，等修复另一个问题后，再回到工作现场  
 
 1. 首先```git stash ```存储本地修改到stash暂存区  
 2. 然后拉某个分支修改bug
 3. bugfix成功后再切回暂存的分支 ``` git checkout 分支```
 4. 查看工作区状态```git status ```
 5. 恢复分支并删除stash内容```git stash pop ```
 6. 使用```git stash list```查看是否删除干净

 

# 远程仓库 
* 本地库与远程库的关联  
  1. 使用SSH把本地仓库与远程仓库进行关联
  2. 在远程仓库创建一个Git仓库
  3. 查看远程库信息``` git remote -v```
  3. 本地clone仓库 ``` git clone 远程仓库地址```
  4. 本地仓库推远程仓库 ``` git push -u origin 分支name```


# 分支管理
* 基本分支操作  
```
查看分支：git branch```  
```创建分支：git branch <name>```  
```切换分支：git checkout <name>```  
```创建+切换分支：git checkout -b <本地分支name> origin/<远程分支name>```
```提交分支修改：git add /git commit```
```切换到master分支：git checkout master```  
```合并某分支到当前分支： git merge <某分支>```. 
```删除分支：git branch -d <name>``` 
git分支就是每次提交串成的一条时间线。在git里HEAD的指向当前分支的指针，如创建新分支Dev时，就是创建了一个指向master的指针，同时HEAD指向了Dev分支。
* 提交分支到远程仓库  
  1. 推送修改```git push origin branch-name ``` 
  2. 如推送失败，则更新本地分支```git pull ```
  3. 如提示 no tracking information 则需建立本地分支与远程分支的链接关系``` git branch --set-upstream-to <branch-name> origin/<branch-name>```
  3. 如合并有冲突解决冲突，本地提交
  4. 再次推送

* 解决冲突
当git无法合并时需先解决冲突，再提交合并。
``` git log --graph```查看分支合并图。

* 合并模式
git merge的模式分为：普通模式 和 fast forward。 fastforward合并后，如删除某个分支，会丢失该分支信息。故在合入代码时可使用``` git merge --no-ff -m'description'```命令切换到普通模式。
* 拉取分支时提示“fastline导致错误”，可使用```git submodule deinit fastlane ```

