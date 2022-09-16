git init 

git remote -v 

git remote add origin git@github.com:gw20000/mini-vue.git

git push -u origin main    
  // *set up  to track  certain remtoe branch , and the git fetch/push/pull will go by your specified*

git checkout 

git checkout -b 



#  撤销 & 回滚


git reset --hard origin/remote-repo-branchname    //🔥推荐    回滚到远程仓库的某个分支


git reset HEAD^              整体撤销 上一次commit操作  只是撤销动作 文件修改不变

git reset --hard HEAD~1      同上   //🔥推荐

git reset HEAD^  filename    针对某一个文件， 功能同上

git reset --hard HEAD^       撤销之前未add的所有修改（回滚到 上一个版本 并 同步到工作区间）



## 撤销 add 操作  （从暂存区吐出来）
##  如果.ignore doesn't work  / 整体 撤销add操作  （清空暂存区） ,重新add

git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master



## 重置git到某一个版本  ( go a certain existing commit , don't worry about using reset ,because  commit record will never  truely disppear in git )

git reset --hard HEAD^：回退到上一版；
git reset --hard HEAD^^：回退到倒数第二版；
git reset --hard 3628164：回退到commit id为3628164的版本；   //🔥推荐 






# git branch 分支命令详解
git branch （查看本地分支）
git branch -r （查看远程分支）
git branch -a （查看所有分支）
git branch < branchName > （创建本地分支）
git branch --set-upstream-to=origin/< branch > feture-test （建立本地分支与远程分支的联系）
git branch -m old new / git branch -M old new （重命名分支）
git branch -d branchname / git branch -D branchname （删除本地分支）
git branch -d -r branchname （删除远程分支）



# 删除远端分支

先查看git项目列表：git branch -a

eg: remotes/origin/xxx

获取所有的列表，然后切换到需要删除的分支：git checkout xxx

最后删除该分支：git push origin --delete xxx



# about  config  

 git config --global user.name "KKK"
 git config --global user.email "changjiu1391@126.com"

 可以用git config --list 查看当前的设定
软件工程师的一大美德就是懒 ，抛弃重复机械的工作
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.l "log --oneline --graph"

当然，这些都可以在~/.gitconfig中进行修改


## 先把文件添加进暂存区 

如果新建了一个文件，那么就会提示Untraced files，就是说这个文件还没有  /*被加入Git版控系统中，并没有被Git正式追踪*/，仅仅是检查到有这个东西；

 a file  in  git store , has   3 kinds status :  untracked  ,  cached ( the file has been put in stagging area)   ,  staged 

再从暂存区提交到仓库中

这个操作结束后，Git就把暂存区的东西储存到了仓库（Repository）中，也就是说：我Git完成了一个文件的存档/备份，也就是做了第一份的版本建立。


Commit到底做了啥？

其实它就是负责了暂存区的东西，也就是说，我们在执行git commit的时候，那些没有添加到暂存区中的档案，就不会被Commit到仓库中


Commit可不可以提交“空”？

因为在R里，空也是被当成字符来处理的（虽然很绕，它为空，应该没东西，但却还占据着位置）。那么Commit其实也是可以的，但是除了作演示Git的合并以外一般没啥用，可以使用git commit --allow-empty -m "empty" 提交一个“空”文档，然后描述就是“empty”

三大重要空间
Git中的三大重要空间就是：**工作区（Working directory）、暂存区（Staging area）、仓库（Repository）**，它们的关系是：


那么就有一个问题：难道所有的流程都要通过add、commit后才能完成吗？能不能简化流程？

其实可以在commit的时候加上一个参数，如: git commit -a -m "update"，但是这个参数只对已经在仓库中的文件有效，对新加入的文件（即Untracked）的文件无效

其实用分段式管理还是不麻烦的，并且对所有文件通用：可以想象有个仓库，仓库门口是个中转站，中转站上有货物有进有出，要存放到仓库的货物可以先放到中转站（git add），然后等待货物达到一定数量了，就打开仓库门，把中转站的货物一起运进仓库（git commit），并且对每一批货物都做好记录（**干嘛的、谁送的**）

为什么要攒一段时间再开一次仓库门呢？

其实来一件货物开一次门是可以的，但是问题就是记录次数太多，让Commit记录变得太零散，如果让别人看的时候，大家希望看到一个比较完整的内容，而不是一个Commit接着一个看

因此，问题又来了：**什么时候选择进行Commit比较好？**

没什么硬性规定，当完成一个任务时或者一天紧张的工作学习时光结束时，可以备份也作为记录

## 保持看记录的习惯
回顾一般的操作流程：

touch a.text     // create a new file 
git add a.text //  add a.text into  stagging  area
git commit a.text  //  excute commit 

查看之前的记录是用git log ，其中会包含几项重要信息:

Commit作者是谁 ? 
什么时候进行的commit ?
每次Commit都做了什么 ?

另外，还会看到commit cc797cdb7c7a337824a25075e0dbe0bc7c703a1e 这种信息，看似乱码，其实是非常关键的内容：它其实是SHA-1 （Secure Hash Algorithm 1）演算得到的结果，特点就是重复率极低，因此可以当做Commit的ID号【一般使用6-8位就足以区分不同的Commit了】

当然，**如果在git log后加上--oneline --graph，结果就会变得更精简，一次显示的结果更多**


## 利用log信息，我们可以做的事情可以包括：

查找某个人的Commit（团队协作中很重要）
git log --oneline --author="doodle"
查找包括某个字符的
git log --oneline --grep="btw"
还可以对Commit的文件进行查找，比如想找提到“生信星球”的文件
git log -S "生信星球"
查询历史资料时，可以限定时间
git log --oneline --since="9am" --until="5pm" --after="2018-12"




# 不可避免的修改
在Git中，不管是删除文件还是重命名，对Git都是一种 【修改】

关于修改的部分，比较零散，我在前面都加了编号，表示不同的操作

## 删除文件

方案一 自己删除自己提交
可以把原来文件直接rm a.txt删除，看下状态，就会提示deleted: a.txt。但这只是第一步，只是说明我们做了更改，并不是本地删除Git存档中就删除。但是还是要继续往下走

rm  a.txt

git st

git add . 

git cm 'delete a.txt'


方案二 Git帮忙删
使用git rm a.txt可以省去add的步骤，然后直接commit就好了


## 只想把文件打入冷宫？
上面两种办法都是直接删除文件（直接kill掉），万一我们只是想把文件从Git中移出来，打入冷宫，并不kill它呢？

使用--cached参数即可

例如：git rm a.txt --cached ，提示信息会显示：rm a.txt。实际上，并不是真的把文件删了，而是把文件从Git中剔除，如果此时再查看一下状态，就会发现变成了Untracked



## 想变个名字时尚一下？
例如想把a.txt改为b.txt，也是可以用两种方案：

方案一 自己改名
mv a.txt b.txt ，注意：虽然只有一个更改的动作，但是Git完成了两个：一个是删除a.txt文件，另一个是新建了b.txt文件。因此最后的状态变成了Untracked。接下来，只需要用git add --all，就会发现现在的状态变成了renamed: a.txt -> b.txt

方案二 Git改名
利用git mv a.txt b.txt，也是想删除操作一样，可以少一步操作直接变成renamed状态

事实上，Git并不在乎你取什么名字，它是根据文件内容计算得到SHA-1值，然后当我们更改文件名时，Git的第一步只是指向了原来的文件，当文件名改变后，Git才又为它重新生成一个新的编号/名称



## 想修改Commit记录？    ( 注意，只是提交记录 即 -m 后面的文本 ，但仍会重新 生成一个commit ID （SHA-1）) 
如果有时因为情绪向Commit中掺杂了个人情感，事后感到后悔想修改下，一般有这几种方法（严重性从大到小排列）：

整个删除.git
使用git rebase修改
先用git reset拆除commit，整理后再重新commit
利用--amend参数修改最后一次Commit

修修补补最后一次Commit amend
加入原来的log中有一句：


$ git log --oneline
3879515 GUN
5dbc437 add a.txt
357fce7 add container
adb4f43 update index 

现在感觉第一句不太恰当，想要做下修改，于是可以

$ git commit --amend -m "Good Ur Night"
[master 414a91c] Good Ur Night

想要修改更早的Commit =》 Rebase
amend只能修改最后一次的记录，但是Rebase可以修改更早的

切记： 上面的例子可以看到，虽然我们只是改了下语言，但是最后的SHA-1值就发生了变化，于是就相当于改变了时间线【看过”闪电侠”或者“蝴蝶效应”的朋友都知道改变时间线的后果】。因此，尽量不要在push后重新修改


## 什么？记录到一半，又有新任务？
我们的学习过程都是并行的，可能同时手头有多个任务，如果自己在做一项，同时又有新任务来，并且很紧急，那么就需要切换到重要级优先的任务中去。

先不管那么多，先保存目前的所有修改git add -all ，然后Commitgit commit -m "not finish yet" ，接下来就可以切换到其他分支去工作，做完后再切换回之前的分支，最后Reset一下git reset HEAD^


## 已经Commit完，却发现还有一个文件忘了加
一般会碰到多个文件一起Commit，但完成后发现忘了一个，这时情况就比较尴尬：不想花一份同样的时间却只为了一个孤零零的文件；但是这个文件不添加又怕后来忘记。于是可以。。。

使用git reset撤掉最后一次的Commit，加入新文件后重新Commit

使用--amend进行Commit

例如：文件c.txt是被落下的，可以先git add c.txt，然后用git commit --amend --no-edit 就可以吧c.txt并入最后一次Commit【--no-edit表示我不想编辑Commit信息，所以也不要跳出来VIM编辑器了】

还是一样，push前需要检查清楚，尽量不在push后重新修改


## 可以只Commit文件的一部分吗
在git add时可以就加上-p参数，然后Git就问你需不需要把这个区块（hunk）加入暂存区，接下来选择y就表示将整个档案加入；选择e就弹出编辑器，让你自己删除那些不想添加的区域，保存后离开，这部分就被加到暂存区了


## Git对空目录是不认识的
比如新建一个目录，想要Commit，但是git status 始终显示工作目录没有发生任何修改，这是因为Git是针对文件内容进行计算的，只有空目录没有内容，它感受不到目录存在。解决办法就是：touch DIR/.keep 在新目录下新建一个隐藏的文件就好，然后就继续add +commit 就可以


## 误删了文件怎么办？
很多情况下（尤其精神不好的时候），容易冲动用rm搞砸一切，假入不小心删除了Git目录下的一些文件，此时的状态都应该是deleted。通过git checkout c.txt 可以救活一个，想全部救活就用git checkout .

上面的操作全都是恢复到上一次commit操作，如果想恢复更远（例如两个版本以前），就可以加个参数：git checkout HEAD~2 c.txt


## Git中经常见到的HEAD是什么东西
**在Git的逻辑中，HEAD相当于一个风向标，指向某个分支，而分支指向某个Commit。一般可以认为HEAD是“目前所在的分支” 。**HEAD信息存放在.git目录下 ，cat .git/HEAD得到ref: refs/heads/master 信息。其中可以看到，目前HEAD指向master分支，看一下master分支中都有什么：cat .git/refs/heads/master ，结果就是这样的字符：ecdfbac9f5fe463e078fb4f737ce39773895e121

因此，如果我们有多个分支（例如有master、doudou、huahau），想切换的话，可以用git checkout 。在变动的时候，随之改变的还有.git/HEAD文件和Reflog文件



## 修改历史信息   (edit history commit record msg , this will not change code.)
感觉就像坐着时光机穿越似的，利用rebase这个时光机可以回到过去，修改多个历史的Commit信息

先看看当前的记录：git log --oneline ，然后选择一个时间节点 git rebase -i bc08d2（其中-i是互动模式），之后会打开一个编辑器 **(git vim 编辑器： i进入编辑模式 esc退出编辑模式 ZZ 保存并退出 )**，其中开头的pick 意思是保留Commit，不做修改。找到想改的Commit行，把pick改成reword，保存离开。接着就会跳到我们要改的那个Commit的编辑器中，更改完信息后，保存退出；进行下一个...【需要注意的是：在互动模式中，文件从上到下是由旧到新，正好与log记录中相反】

如果改着改着，中途不小心退出了，不用担心。其实Git是为你保留了记录的，接下来选择git rebase --continue就是继续，选择--abort 就是彻底退出之前的编辑。



我们改完后，如果想回到修改之前，需要用到git reset ORIG_HEAD --hard



 ## 合并臃肿的Commit信息
有时候，我们在提交Commit信息时，难免有些复杂，比如一个文件写add doodle 1 ，另一个又写add doodle 2 ，一个Commit的利用率不高（这里的一个Commit只对一个文件有效，造成了最后log文件的繁琐）。如果可以将多个相似的Commit进行合并，是不是能清爽一些？

像上面一样，还是先git log --oneline ，再git rebase -i bc08d2 ，只不过对于要合并的Commit的信息将pick改成squash ，最后在弹出的编辑器中进行编辑，保存就结束了





## 撤销三兄弟
Reset （直接回滚/改变历史commit记录）： 改变历史记录【把目前状态设成某个指定的Commit状态，一般用于未push的Commit】
Rebase（时光机/改变历史commit记录）： 改变历史记录【新增、修改、删除Commit都很方便，一般用于未push的Commit】
Revert（反转操作/间接反做/间接撤销/不改变历史commit记录）：不改变历史记录【新增一个Commit来对之前的Commit做反转，当然原来的Commit还是会留在历史记录中，**一般用于push过的Commit**】






## 想要撤销Commit =》Reset
例如现在的log包含了以下信息
 git log --oneline
f12d8ef (HEAD -> master) add c.txt
83e7e30 add b.txt
687fce7 add a.txt

现在想要撤销最后一次的Commit，就可以用git reset f12d8ef^【其中这个^表示前一次的Commit，如果是要到前两次就^^，次数再多就直接f12d8ef~5 表示5次】。但是如果是返回HEAD这个Commit的话，还有种简单的方法可以表示：就是git reset master^或者git reset HEAD^ 。

因此，如果想回到687fce7，就可以：git reset 687fce7、git reset HEAD~3、git reset master~3 。第一种就像绝对路径，而后两种有点相对路径的意思。

另外，reset还有三个参数，它们还是有一些不同的，主要区别就是对从Commit中撤销回来的文件怎么处理

模式	--mixed（默认）	--soft	--hard
从Commit撤销的文件去向	丢回工作目录	丢回暂存区	直接丢弃
虽然我们知道，reset的英文含义是“重置”，但是在Git中更像“become”或者“go to”的意思，表示“去到哪里”。因此即便用了reset也不要有什么心理压力，既然表示“去到哪里/变成什么样子”，那我们也可以撤回，都不是事。不要认为用了reset就和恢复出厂一样的严重

例如：这里如果使用—hard参数从f12d8ef退回到687fce7，git reset HEAD~2 --hard ，那么暂时的结果就是只保留了687fce7的文件，其余全部消失。   如果想恢复到reset之前的状态，     把所有的文件再重新恢复，可以看一下Git中的git reflog(里面存放了对HEAD的每一次改动信息)，看看之前的SHA-1值是多少，查到是f12d8ef，于是就可以用git reset f12d8ef --hard 把刚才reset的文件都捡回来






## revert 间接撤销

团队协作开发中常遇到这样的场景 ，最近一个或多个commit 之前 有一个或几个 commit 需要 去掉， 但是显然，这些分支都已经push远端分支 并 合并到了远程主干分支 ，那么，显然， 主干分支直接回滚不太现实， 否则，最近的版本都要重新操作 

（在 Git 开发中通常会控制主干分支的质量，但有时还是会把错误的代码合入到远程主干。 虽然可以直接回滚远程分支， 但有时新的代码也已经合入，直接回滚后最近的提交都要重新操作。 那么有没有只移除某些 Commit 的方式呢？可以一次 revert操作来完成。）

下面的部分就开始介绍具体操作了，同时我们假设远程分支是受保护的（不允许 Force Push）。 思路是从产生一个新的 Commit 撤销之前的错误提交。

使用 git revert <commit> 可以撤销指定的提交， 要撤销一串提交可以用 <commit1>..<commit2> 语法。 注意这是一个前开后闭区间，即不包括 commit1，但包括 commit2。


* 982d4f6 (HEAD -> master) version 6
* 54cc9dc version 5
* 551c408 version 4, harttle screwed it up again
* 7e345c9 version 3, harttle screwed it up
* f7742cd version 2
* 6c4db3f version 1



git revert --no-commit f7742cd..551c408
git commit -a -m 'This reverts commit 7e345c9 and 551c408'

其中 f7742cd 是 version 2，551c408 是 version 4，这样被移除的是 version 3 和 version 4。 注意 revert 命令会对每个撤销的 commit 进行一次提交，--no-commit 后可以最后一起手动提交。


（注意 使用 参数  -no-commit 的好处  ，  然后最后一起手动提交一次即可， 因为 git revert 对每个反转的commit都会重新commit一次）

此时 Git 记录是这样的：

* 8fef80a (HEAD -> master) This reverts commit 7e345c9 and 551c408
* 982d4f6 version 6
* 54cc9dc version 5
* 551c408 version 4, harttle screwed it up again
* 7e345c9 version 3, harttle screwed it up
* f7742cd version 2
* 6c4db3f version 1


现在的 HEAD（8fef80a）就是我们想要的版本，把它 Push 到远程即可。

wo:  revert  和  cherry-pick 的 作用有些重复了




 ## 打个Tag
一般当我们认为自己的代码完成了里程碑式的发展时，比如开发版本1.0.0或者beta发布，一般会打个tag

分为轻量标签（lightweight tag）和附注标签（annotated tag）

官方说明：Annotated tags are meant for release while lightweight tags are meant for private or temporary object labels

轻量标签：相当于一个贴纸，直接粘上就完事，例如git tag douhua 347dfe
附注标签：含有的信息会多一些，像一个简短的介绍，例如git tag douhua 347dfe -a -m "Doudou and Huahua" (-a 表示 annotation；-m 和Commit中的一个意思)
删除标签使用-d参数，例如git tag -d douhua

既然标签和分支都充当风向标，那么有什么不同吗？

当Commit修改时，分支是随之移动的，而标签还在原地，牢牢贴在那里

查看tag
git tag     //  查看有哪些tag
git show <tag-name>   // 查看tag对应的版本号

git reset --hard  <commit-id>


git tag -d <tage-name>

git push origin :refs/tags/V20210310

或  git push origin --delete tag V20210310




# 分支？分身？
假入你有一个分身，你要让他干嘛呢？

答：帮你吓退敌人；帮你学生信；帮你写作业；帮你做实验。。。这些确实都是不错的选择，但是分身有个最大的作用就是：一旦任务执行失败，本体并不受影响

如果我们的任务比较复杂，尤其一个package需要多个人共同完成时，就不能随便Commit。我们可以在另一个分支中来测试某些新功能或者修复某些bug，当效果满意时我们可以选择合并到主线；即便效果不好，也不会对主线造成影响





## 新接触一个分支
查看分支很简单，直接git branch就好，预先设置的都是master分支，当前分支前面有*标明

想要增加一个分支，可以直接加上名字git branch doodle ，这样就增加了一个doodle的分支

分支名字不够霸气？改！例如有一个分支叫紫薯，我们完全可以用git branch -m 紫薯 紫薯侠 ，即便是预先安装的master也可以改

## 删除分支也是可以的：
使用git branch -d test ，但是这里如果test分支还没有合并到主线，删除时会提醒你。如果真的不想要他了，并且不要合并，可以用-D强制删除。因此，所有的分支都能删，包括master，只不过不能删当前的分支（就像conda删除环境一样，不能删除当前所在的）

想要切换分支？ 直接git checkout doodle ，随后星号就移到了doodle上面


## 什么？切换分支文件丢失！
加入我们在doodle分支下新建了两个文件，并且add+commit 后也确实看到文件在列表中；但是如果切回原来的master分支，发现新建的两个文件没了，其实不用担心，文件还在，只不过在当前分支显示不出来。我们只需要切换回原来的doodle分支就好了


## 切换到不存在的分支可以么？
一般来说，我们都要先建立一个分支，然后再切换。但是如果想切换到一个之前没有建立过的分支，有一个快速的办法就是git checkout -b 新分支名字 【-b参数的作用就是：如果切换的分支存在，就直接切换；如果不存在，就新建一个分支，然后切换过去】

或者可以利用这个命令，在之前的某个Commit来新建一个分支，感觉就好像 ”回到年轻时，做点不一样的事“，例如之前有一个Commit 347dfe4值，我们想回到这里新建一个huahau的分支：git branch huahau 347dfe4


## 分支合并
如果我们之前新建了分支doodle，并且在doodle中Commit了两个文件，然后想要回到master分支，把doodle中的文件合并过来，可以用：git checkout master + git merge doodle ，这时master中就会出现doodle中的两个文件。

wo :  就是把别的分支 新增的commit拿过来  （如果 有villision ，auto merge will fail说明 ， 当前分支 与 被合并分支 的历史commit有差异，看你想要取舍谁了， 手动解决冲突即可 ,然后手动 add + commit一次）

## 那么合并完的分支还要继续留着么？

其实按道理讲，主线master已经拥有了doodle的一切，那么doodle就没有作用了。但是如果不想让自己对亲分身有”过河拆桥“的愧疚的话🥺，留着也不是不行。完全看自己



## 没有合并的分支不小心被砍掉，怎么办？

合并完的我们可以随心意，但是如果使用了git branch -D强行删除没有合并的分支，就会给你一个信息（Deleted branch doodle (was c274a5a).）,其中那串Commit字符SHA-1值很重要，就是你的”后悔药“。我们知道，**分支只是一个指向某一Commit的风向标，删除这个风向标并不会导致Commit消失**

于是想要恢复分支，可以利用原来的Commit SHA-1值，用git branch new_doodle c274a5a 来迎接老朋友【还是那句话，SHA-1值不需要刻意记忆，需要去到Reflog中查询】

## 只要某个分支的某几个Commit
使用cherry-pick 加上想要的Commit号，就把那个Commit内容复制过来，于是在当前的分支上就多了一个commit。加多个Commit就在命令后多写几个Commit号。

默认情况是复制过来就合并到当前分支的，如果不想合并只放在暂存区，就用git cherry-pick --no-commit

wo :  git  br  <brname> <commmit-id>     
      git cherry-pick   <one or a couple of commit-ids>



## 什么？记录到一半，又有新任务？
我们的学习过程都是并行的，可能同时手头有多个任务，如果自己在做一项，同时又有新任务来，并且很紧急，那么就需要切换到重要级优先的任务中去。

先不管那么多，先保存目前的所有修改git add -all ，然后Commitgit commit -m "not finish yet" ，接下来就可以切换到其他分支去工作，做完后再切换回之前的分支，最后Reset一下git reset HEAD^





# GitHub


## 让本地与远端保持同步
有时我们会在网站上直接更改，如果本地想保持最新，可以用 git pull --rebase 。我们知道，这也算是一个合并的任务，因此也会在本地默认加上Commit (wo : 只不过是merge远端分支, 3种的一种或2种：already up to date ; 有新增commit ，有冲突villision )，加上rebase的目的就是不要加这个关于合并的Commit


wo :  加上了 --rebase  ，变基：改变当前分支 创建时（出生时/ 克隆分支时）的基点 


wo :  fetch 拉下来 ， 是为了 同步远端commit（git fetch 抓取远程主机（远端版本库）上所有分支上新增的commit）到本地版本库
      push  推上去 ，是为了 同步本地版本库 某个分支上 新增的commit  到 远端版本库   



## 本地推不上去什么鬼？  (don't recommend to  git push -f , cos it will probably overwrite team player's commit ， so use prudently )
有时本地push会报错

git push
To https://github.com/eddiekao/dummy-git.git
 ! [rejected]        master -> master (fetch first)


这是因为远端比本地的版本还新，Git不想这么做，解决办法如下：

先拉再推，就是先从远端pull到最新版git pull --rebase ，如果合并没有冲突，再往上推

甚至还有一种：git push -f使用蛮力往上推，造成的影响就是可能会把共同开发者的版本覆盖掉，导致他们资料丢失【一般不用！】






## 网上发现某人写的不错，想拷贝到本地看看
使用git clone，可以选择HTTPS或者SSH，如果clone下来想保存不同的名称，可以在命令后面加目录名

git clone xxx doodle ，于是就把xxx上的整个内容（包括历史记录、分支、标签等）复制到本地一份，保存为doodle

一般来讲，clone命令只需要用一次，并且命好名字，之后想追更新，直接用pull就好



## git push 命令用于从将本地的分支版本上传到远程并合并。
 git push <远程主机名> <本地分支名>:<远程分支名>    // wo：远程主机名 ： 远程仓库名  或者 远程仓库地址 
 如果本地分支名与远程分支名相同，则可以省略冒号：
 git push <远程主机名> <本地分支名>


git push origin master === git push origin master : master 

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

git push  --force origin master

以下命令表示删除 origin 主机的 master 分支：

git push origin --delete master 


## Git——在 Git 中设置 Upstream    (为 当前分支（本地分支）设置 上游分支/远程跟踪分支/upstream分支/远程关联分支/远端跟踪分支)


设置上游分支/设置远程跟踪分支/设置upstream分支 /设置远程关联分支， 在我们克隆分支或新建一个仓库之后， 在推送/获取 之前，必须先设置好 上游分支 ，否则，你是无法push/pull/fetch 的 

参数 -u 是 --set-upstream的简写

设置 Upstream： 

git branch --set-upstream <remote-branch>

git branch -u  <remote-branch>

例如
git branch -u origin/test    (远端test分支必须存在)



此外，还提供以下提到的选项：

使用 Git Push 设置 Upstream 分支

git push -u <remote> <branch>

例如 git push -u origin test (same as  git push -u test:test 将本地test分支 推送到 远端test分支 ，并将远端test分支（如果远端不存在test分支就在远端新建test分支） 作为 当前分支（本地test分支）的上游分支)

 git push -u origin test:test2 (将本地test分支 推送到 远端test2分支 ，并将远端test2分支（如果远端不存在test2分支就在远端新建test2分支） 作为 当前分支（本地test分支）的上游分支)

例如  git push -u origin main   // 将 当前分支推送 到 远端 main分支，并将远端main分支 设置为 当前分支 的 上游分支/远程跟踪分支/远程关联分支


这将轻松地为任何**未来的任何推送或拉取命令**设置 Upstream 关联




## 小叉子Fork用起来
Pull Request （PR）过程就是一个相互交流、共同开发的过程，事情是这样的：

先是从GitHub上看到一个作者写的东西，自己感兴趣，于是自己先Fork一份原作者的文档到自己的账户下
自己拥有所有修改权限，想怎么动就怎么动
改完后，push回自己的账号，然后给原作者发通知，说你帮他搞了一点新功能，请查收
作者如果满意，就会把你的工作merge到他的文档中


## 想要删除远端的分支
比如本地的分支是master，远端服务器地址是origin表示【如果要本地修改远端仓库名称，可以用git remote rename; 或者修改远端仓库的URL： git remote set-url origin url 】

复习一下：如果我们要将本地的文件推到远端，并且建立远端分支doodle，可以用git push -u origin master:doodle 就是说，将本地的master推上去后，本来默认是在远端建立一个相同名字的分支，但现在指定了叫doodle

这个origin怎么来的呢？是利用git remote add得到的，如果我们命名为huahua，那么也可以用huahua表示远端服务器的地址

现在我们有了远端分支doodle，那么怎么删除它？

其实我们只需要把原来push操作中的本地分支变成空，就相当于删除了远端分支。也就是说，上传一个空的分支到远端git push origin :doodle

或者 git push  origin -d  doodle 


## 查看本地和远程的跟踪关系  （查看当前远程跟踪分支）

git branch -vv



## git fetch  (取回所有分支的更新 / 将某个远程主机的更新，全部取回本地 /  抓取最新的远程跟踪分支 到 本地版本库中 )   git fetch origin branchname  取回特定分支的更新（/抓取指定的最新的远程跟踪分支 到 本地版本库）（ git fetch <远程主机名> <分支名>  ）

 fetch 抓取到新的远程跟踪分支时，本地的工作区（workspace）不会自动生成一份可编辑的副本，抓取结果是直接送到版本库（Repository）中。


如果想要在 origin/master 分支上工作，可以新建分支 test 并将其建立在远程跟踪分支之上：

git checkout -b test origin/master

这会给你新建一个用于工作的本地分支 test，并且起点位于 origin/master。       如下图：现在的本地版本库

https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azMzcnp2b3FqMzBtbDBiaW14Zi5qcGc

git fetch origin master 后 本地版本库 如下图 ：

https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azM3Ync4OHJqMzBtbTA5b3dlcC5qcGc

git fetch  会拉取最新的远程跟踪分支 到 本地版本库中  ， 但不会改变工作区 和 自动合并 提交，所以， git fetch  是 git fetch origin 简写  ，表示 将某个远程主机的更新，全部取回本地 ， 默认情况下，git fetch取回所有分支的更新。如果只想取回特定分支的更新，可以指定分支名,如下所示 -

 git fetch <远程主机名> <分支名>

取回origin主机的master分支。

git fetch origin master 

所取回的更新，在本地主机上要用”远程主机名/分支名”的形式读取。比如origin主机的master分支，就可以用origin/master读取。

$ git branch -a
* master
  remotes/origin/master

  上面命令表示，本地主机的当前分支是master，远程分支是origin/master。




HEAD时分支的风向标 ，分支时commit的风向标 

克隆仓库 or 克隆分支尤其是克隆远程分支 时，会有一个基点 ， 在此基点上工作，提交会在本地版本库分支上增加新的commit 

将来，我们需要，在本地分支上 合并 远程跟踪分支上 别人提交的commit ，    可以使用 git  merge  origin/master    也可以使用git rebase  origin/master

合并后结果（commit图）（git rebase origin/master 与 下图不同，下图是git merge origin/master的，但是版本都是一样的，只是路径不同，可以说是，‘殊途同归’ ）如下图 https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azE3enBuaHhqMzBvaTA5dWFhYy5qcGc

 还可以 ， git merge origin/master --rebase  (这个commit图，又会不一样，但，仍是 “殊途同归”) 如下图
 https://img-blog.csdn.net/20160420134814629


 在 rebase 的过程中，也许会出现冲突(conflict). 在这种情况，Git会停止rebase并会让你去解决 冲突；在解决完冲突后，用" git-add "命令去更新这些内容的索引(index), 然后，你无需执行 git-commit,只要执行:
$  git rebase   --continue
这样git会继续应用(apply)余下的补丁。
在任何时候，你可以用--abort参数来终止rebase的行动，并且"mywork" 分支会回到rebase开始前的状态。
$  git rebase   --abort

 如果在git pull的时候加上rebase参数，即git pull --rebase,这里表示把你的本地当前分支里的每个提交(commit)取消掉，并且把它们临时 保存为补丁(patch)(这些补丁放到".git/rebase"目录中),然后把本地当前分支更新 为最新的"origin"分支，最后把保存的这些补丁应用到本地当前分支上。

从commit图上可以 清晰看到  ：  克隆仓库 ，克隆分支 ， 本地新增commit ， 远端分支新增commit ,取回所有分支的更新/或 取回特定分支的更新 ，本地分支合并远端分支      git clone   git branch   git commit   git fetch/或 git fetch origin xxx  git merge  origin/xxx 


git rebase --abort   //取消rebase 



//参考内容：
//https://www.yiibai.com/git/git_fetch.html

// https://blog.csdn.net/duomengwuyou/article/details/51199597

