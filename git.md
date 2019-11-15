# cheatsheet-git

>https://www.liaoxuefeng.com/wiki/896043488029600


## 工作区&&缓存区 
git只负责将缓存区中的修改commit到repository 工作区的修改不会被commit                                       

工作区相当于本地文件 缓存区相当于repositiory的缓存            



----

## 基本命令

* config                
git config --global user.name "username"               
git config --global user.email "useremail@example.com"            

* repository            
git init  添加repo

* add to repository         
git add filename  将文件从工作区加入缓存区       
git commit -m "comment" 将缓存区的修改添加到repository 最好添加注释

* check status      
git status  查看工作区缓存区状态   
git diff HEAD -- filename 查看历史版本与当前版本区别       

* time machine      
git reset --hard HEAD~10    HEAD指向的版本就是当前版本      
git reset --hard commit_id
git reflog  查看命令历史 以便确定要回到未来的哪个版本                  
git log --graph --pretty=oneline --abbrev-commit  查看提交历史 分支合并情况等
git checkout -- filename 工作区的修改全部撤销                       
git reset HEAD filename 缓存区的修改全部撤销                        
git rm 删除文件

* remote repo   
   * push            
生成id_rsa&id_rsa.pub 将pub放到github上            
ssh-keygen -t rsa -C "useremail@example.com"            
关联本地repo到github的远程repo 先将本地文件夹git init然后才能remote到github                                
git remote add origin git@server-name:path/repo-name.git             
将master push到origin -u将master origin关联                                       
git push -u origin master    


  

   * clone
   git clone git@server-name:path/repo-name.git                
   还可以用https://github.com/michaelliao/gitskills.git                    
   Git支持多种协议 默认的git://使用ssh 但也可以使用https等其他协议。


## 分支管理

git checkout -b dev == git branch dev + git checkout dev 创建并指向dev分支          
git checkout dev 指向dev            
git branch 查看分支                 
git merge dev -m "comment" --no-ff 将dev分支上的改动合并到当前分支中                
git branch -d dev 删除分支 当前指向的分支不能删除 merge不成功的分支也不能删除                      
git branch -D dev 强行删除分支               
git switch -c dev 切换分支 新版本git支持                       
conflict:改动分支dev后commit 再改动master后commit merge就会出现conflict需要解决conflict后在提交                      
--no-ff参数普通模式合并 能看出来曾经做过合并 fast forward看不出来曾经做过合并                 

### BUG分支
修复bug:通过创建bug分支进行修复--->合并--->删除bug分支         
git stash 保存当前工作现场          
git stash list 查看保存的工作现场         
git stash pop 回复工作现场        
git cherry-pick commit_id                
在master分支issue01上修复bug dev分支只需要使用git cherry-pick commit_id就可以将issue01中修复的bug复制到当前dev分支

### 远程分支管理
* 推送修改 git push origin <branch-name>
* 推送失败 git pull 合并   
git remote -v 查看远程库信息                    
git checkout -b branch-name origin/branch-name 在本地创建和远程分支对应的分支          
git branch --set-upstream branch-name origin/branch-name 建立本地分支和远程分支的关联           
git pull 从远程抓取分支 如果有冲突 要先处理冲突    
git rebase 把本地未push的分叉提交历史整理成直线   
* 合并冲突 解决冲突 本地提交
* git push origin <branch-name>


## 标签
git tag <tagname> 新建一个标签
git tag -a <tagname> -m "blablabla..." 指定标签信息
git tag 查看所有标签
git tag <tagname> commit_id 给历史提交的commit_id打标签
git tag -d <tagname> 删除标签
git push origin <tagname> 标签推送到远程
git push origin --tags 一次性推送所有标签到远程
删除远程标签:git tag -d <tagname>先删除本地 git push origin :refs/tags/<tagname>再删除远程



 
error: failed to push some refs to 'git@github.com:diturbed/learngit.git'           
需要将github远程repo pull到本地合并重新提交                 
git pull --rebase origin master 