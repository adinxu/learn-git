# git exercise

## set user information
```
$ git config --global user.name "[name]"
```
对你的commit操作设置关联的用户名
```
$ git config --global user.email "[email address]"
```
对你的commit操作设置关联的邮箱地址
***
## get git repo
### init local repo
```
$ git init [project-name]
```
初始化一个本地仓库，可以指定项目名字，指定后会创建同名文件夹并作为工作目录
若不指定则以当前目录作为工作目录
运行 git init，这会创建一个 Git 仓库，其中的 HEAD 引用指向未创建的分支（master 还不存在）。
git仓库中：HEAD->master->?
### get remote repo
```
$ git clone [url]
```
从远程仓库完全拷贝工程到本地,可以附加名字，即本地库名字
可以加`-o`参数，如
```
$ git clone -o remoterepo [url]
```
那么远程库名字将是`remoterepo`而不是默认的`origin`
clone的本地仓库的分支会自动跟踪远程分支
`--depth=1`选项可以只获取最新的commit
***
## add file to stage
```
$ git add/stage [file]
```
* untracked file add to stage
* modified file add to stage
可以添加文件夹，则会递归添加文件夹下所有文件到暂存
`-i`参数可以查看暂存区内容
`git add . `把当前文件夹中的所有文件
***
## cancel the staged file
```
$ git reset HEAD [file]
```
取消暂存
实际上暂存区将回到上一次提交时的快照内容。也可以直接使用`git reset [file]`。
>暂存各文件只有一份

```
git config --global alias.unstage 'reset HEAD --'
```
为命令取别名，这会使下面两个命令等价：
```
$ git unstage fileA
$ git reset HEAD -- fileA
```
***
## check the file status
```
$ git status
```
可加参数`-s`，查看简化信息
简化信息中：
M 　代表已修改并已提交暂存
 　M代表已修改但未提交暂存
A 代表从未被提交过的文件
??代表未被追踪的文件
***
## check the file diffrent
```
$ git diff
```
* 不加参数代表工作目录中当前文件与暂存区文件间的差异，也就是修改后还没暂存的内容
* 加参数`--staged/cached`
暂存文件与最新版本间(已提交的版本)的不同
* `$ git diff HEAD`显示工作版本(Working tree)和HEAD的差别
* `$ git diff SHA1 SHA2`比较两个历史版本之间的差异

***
## commit the file
```
$ git commit -m"[descriptive message]"
```
将文件快照永久地记录在版本历史中
加参数`-a`则把所有已追踪文件暂存起来一并提交
>多个子参数可以合起来。。。

像这样：
```
$ git commit -am"[descriptive message]"
```
不要傻傻的分开写。。。
***
## check commit history
```
$ git log
```
显示提交的历史
参数`-p`显示提交差异
参数`--graph`以图形表形式显示分支
***
## cancel the file change
```
$ git checkout -- [file]
```
还原文件到暂存状态(若有暂存)或上次提交（无暂存）

***
## remove file from git
```
$ git rm [file]
```
从Git中移除文件
先删除本地文件，再用rm移除
若文件已添加到暂存，则：
* 加参数`-f`强制移除(也将文件从暂存区移出）
* 加参数`--cached`不再追踪文件(也将文件从暂存区移出)，但文件仍保留在硬盘
***

## add remote repo
```
$ git remote add <shortname> <url> 
```
将本地现有仓库关联到远程仓库,默认名字是`origin`，也可以起别名,必须带名字
>只是添加了远程引用，与`clone`不同,`clone`会添加远程引用，为你自动将其命名为`origin`，跟踪远程分支，拉取它的所有数据，创建一个指向它的 `master` 分支的指针，并且在本地将其命名为 `origin/master`。相当于远程仓库的一个副本

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，你可以在任意时间
使用 `-u` 或 `--set-upstream-to` 选项运行 `git branch` 来显式地设置。
```
$ git branch -u origin/master
```
>远程跟踪分支是远程分支状态的引用。它们是你不能移动的本地引用，当你做任何网络通信操作时，它们会自动移动。远程跟踪分支像是你上次连接到远程仓库时，那些分支所处状态的书签。

***
## rename remote repo
```
$ git remote rename [old-name] [new-name]
```
重命名远程库
***
## check the remote repo
```
$ git remote
```
查看远程仓库
加参数`-v`可显示URL
```
git remote show [remote-name] 
```
查看更多信息
***
## pull from remote repo
```
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```
如
```
$ git pull origin master:master
```
表示取回origin主机的master分支，与本地的master分支合并。
如果远程分支是与当前分支合并（即当前为master分支），则冒号后面的部分可以省略:
```
git pull origin master
```
***
## push to remote repo
```
$ git push <远程主机名> <本地分支名>:<远程分支名>
```
1. 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建：
```
git push origin master
```
上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。

2. 如果当前分支与多个主机存在追踪关系，则可以使用`-u`选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`。
反馈如下：
```
Branch master set up to track remote branch master from [remote-name]
```
3. 如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。
```
$ git push --force origin
```
上面命令使用`--force`选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项。
***
## when pull operation report err
```
git pull origin master --allow-unrelated-histories
```
use this command avoid
>refusing to merge unrelated histories
远程库存在本地没有的文件时，`push`会报错，此时`pull`会报上面的错误
***
## fetch from remote repo
```
$ git fetch [remote-name]
```
抓取远成仓库中你没有的数据，但不会自动合并（相较于pull）
当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。换一句话
说，这种情况下，不会有一个新的分支 - 只有一个不可以修改的指针。
所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如`origin`主机的`master`，就要用`origin/master`读取。
***
## remove remote repo
```
$ git remote rm [remote-name]
```
移除远程库，不再关联
***

## list all loacal branch
```
$ git branch
```
`-r`选项，可以用来查看远程分支，-a选项查看所有分支
`-vv`选项查看所有跟踪分支
`--merged`选项查看哪些分支已经合并到当前分支
`--no-merged`选项查看其他分支
***

## creat a new branch
```
$ git branch [branch-name]
```
***

## check the objects which is currently pointed by each branch
```
$ git log --decorate
```
***

## change to another branch and update the work dir
```
$ git checkout [branch-name]
```
加`-b`参数则为创建一个新分支并切换到那个分支

***

## merge specific branch 's history to current branch
```
$ git merge [branch-name]
```
合并冲突时使用`git status`来查看冲突，然后可以打开文件并修改来解决冲突，然后使用`git add`提交暂存来将其标记为冲突已解决
也可以运行
```
$ git mergetool
```
图形化工具来解决冲突

***

## delete specific branch
```
$ git branch -d [branch-name]
```

***
## list tag
```
$ git tag
```
列出标签
```
$ git show
```
可以看到附注标签的标签信息与对应的提交信息
***
## creat an tag
* lightweight
```
$ git tag [tag name]
```
后面可跟`commit id`用来给过去的commit打标签
* annotated
```
$ git tag -a [tag name]
```
加`-m`参数，添加存储在标签中的信息
***
## share tag to remote
```
git push [remote name] [tagname]
```
推送标签
```
$ git push [remote name] --tags
```
推送大量标签
***
## check out tag
```
 git checkout -b [branchname] [tagname]
```
将在在特定的标签上创建一个新分支
***
## delete tag
```
git tag -d [tagname]
```
***
删除本地标签
```
$ git push [remote name] :refs/tags/[tag name]
```
删除远程仓库标签
***

## rebase
```
git rebase [basebranch] [topicbranch]
```
将`topicbranch`中的修改变基到`basebranch`上
如果`[topicbranch]`即为当前分支，则可写为
```
git rebase [basebranch]
```
它的原理是首先找到这两个分支（即当前分支 `[topicbranch]`、变基操作的目标基底分支 `[basebranch]`）的最近共同祖先 ，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底 `[basebranch]`, 最后以此将之前另存为临时文件的修改依序应用。
>一般我们这样做的目的是为了确保在向远程分支推送时能保持提交历史的整洁——例如向某个别人维护的项目贡献代码时。在这种情况下，你首先在自己的分支里进行开发，当开发完成时你需要先将你的代码变基到origin/master 上，然后再向主项目提交修改。这样的话，该项目的维护者就不再需要进行整合工作，只需要快进合并便可。

>无论是通过变基，还是通过三方合并，整合的最终结果所指向的快照始终是一样的，只不过提交历史不同罢了。变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。变基使得提交历史更加整洁。

----------
***不要对在你的仓库外有副本的分支执行变基。***
>变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 git rebase 命令重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你还要拉取并整合他们修改过的提交，事情就会变得一团糟
>总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样，你才能享受到两种方式带来的便利。
***
## rewrite history
### modify the last commit
```
$ git commit --amend
```
直接键入以上命令，可以修改上次的提交信息。若想修改上次快照的提交内容，可以通过修改文件然后运行 `git add` 或 `git rm` 一个已追踪的文件，随后运行`git commit --amend` 拿走当前的暂存区域并使其做为新提交的快照.使用这个技巧的时候需要小心，因为修正会改变提交的 SHA-1 校验和。它类似于一个小的变基 - 如果已经推送了最后一次提交就不要修正它。
### modify more than one commit
为了修改在提交历史中较远的提交，必须使用更复杂的工具。Git 没有一个改变历史工具，但是可以使用变基工具来变基一系列提交，基于它们原来的 `HEAD `而不是将其移动到另一个新的上面。通过交互式变基工具，可以在任何想要修改的提交后停止，然后修改信息、添加文件或做任何想做的事情。可以通过给 `git rebase` 增加 `-i`选项来交互式地运行变基。必须指定想要重写多久远的历史，这可以通过告诉命令将要变基到的提交来做到。
>再次记住这是一个变基命令 - 在范围内的每一个提交都会被重写，无论你是否修改信息。不要涉及任何已经推送到中央服务器的提交 - 这样做会产生一次变更的两个版本，因而使他人困惑。

```
$ git rebase -i HEAD~3

pick 46ad177 3commit
pick 65a142c 2commit
pick 60849fd 1commit

# Rebase 5e482cb..60849fd onto 5e482cb (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

### resort the order of commit
如果想要移除 “added cat-file” 提交然后修改另外两个提交引入的顺序，可以将变基脚本从这样：
```
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```
改为这样：
```
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit
```
当保存并退出编辑器时，Git 将你的分支带回这些提交的父提交，应用 310154e 然后应用 f7f3f6d，最后停止。事实修改了那些提交的顺序并完全地移除了 “added cat-file” 提交。
### split commit
将要拆分的提交的指令修改为 “edit”,当保存并退出编辑器时，会依次应用到你修改的那个commit为止，然后让你进入命令行。那里，可以通过 git reset HEAD^ 做一次针对那个提交的混合重置，实际上将会撤消那次提交并将修改的文件未暂存。现在可以暂存并提交文件直到有几个提交，然后当完成时运行 `git rebase --continue`,Git 在脚本中应用后续提交。
--continue.再一次，这些改动了所有在列表中的提交的 SHA-1 校验和，所以要确保列表中的提交还没有推送到共享仓库中。
### 核武器级选项：filter-branch
暂时用不到
***
## reset
|树               |用途                              |
|:----------------|:--------------------------------|
|HEAD             |上一次提交的快照，下一次提交的父结点|
|Index            | 预期的下一次提交的快照            |
|Working Directory|沙盒                              |
***
**HEAD**

HEAD 是当前分支引用的指针，它总是指向该分支上的最后一次提交。这表示 HEAD 将是下一次提交的父结点。通常，理解 HEAD 的最简方式，就是将它看做 你的上一次提交 的快照。
**索引**

索引(index)是你的预期的下一次提交。我们也会将这个概念引用为 Git 的 “暂存区域”，这就是当你运行 gitcommit 时 Git 看起来的样子。

**工作目录**
最后，你就有了自己的工作目录。另外两棵树以一种高效但并不直观的方式，将它们的内容存储在 .git 文件夹中。工作目录会将它们解包为实际的文件以便编辑。你可以把工作目录当做 沙盒。在你将修改提交到暂存区并记录到历史之前，可以随意更改。
***
***reset***
reset的三种基本操作：
第 1 步：移动 HEAD
```
git reset --soft HEAD~
```
reset 做的第一件事是移动 HEAD 的指向。这与改变 HEAD 自身不同（checkout 所做的）；reset 移动HEAD 指向的分支。无论你调用了何种形式的带有一个提交的 reset，它首先都会尝试这样做--使用 `reset --soft`，它将仅仅停在那儿,不会改变索引和工作目录。。
第 2 步：更新索引（--mixed）
```
 git reset HEAD~
```
reset 会用 HEAD 指向的当前快照的内容来更新索引。如果指定 --mixed 选项，reset 将会在这时停止。这也是默认行为，所以如果没有指定任何选项（在本例中只是 git reset HEAD~），这就是命令将会停止的地方.
第 3 步：更新工作目录（--hard）
```
 git reset --hard HEAD~
```
这将撤销最后的提交、git add 和 git commit 命令以及工作目录中的所有工作。必须注意，--hard 标记是 reset 命令唯一的危险用法，它也是 Git 会真正地销毁数据的仅有的几个操作之一。
其他任何形式的 reset 调用都可以轻松撤消，但是 --hard 选项不能，因为它强制覆盖了工作目录中的文件。若想找回撤销前的提交，可以通过`reflog`来找回，但未提交的修改部分则无法恢复了。
### 通过路径来重置
运行 git reset file.txt（这其实是 git reset --mixed HEAD file.txt 的简写形式，因为你既没有指定一个提交的 SHA-1 或分支，也没有指定 --soft 或 --hard）.它本质上是将file.txt 从 HEAD 复制到索引中。我们可以不让 Git 从 HEAD 拉取数据，而是通过具体指定一个提交来拉取该文件的对应版本。我们只需运行类似于 `git reset eb43bf file.txt `的命令即可。还有一点同 git add 一样，就是 reset 命令也可以接受一个 --patch 选项来一块一块地取消暂存的内容。这样你就可以根据选择来取消暂存或恢复内容了。
```
Apply this hunk to index [y,n,q,a,d,/,e,?]? ?
y - apply this hunk to index
n - do not apply this hunk to index
q - quit; do not apply this hunk or any of the remaining ones
a - apply this hunk and all later hunks in the file
d - do not apply this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```
需要注意的是若针对文件，则HEAD指向不会改变，否则HEAD指向也会改变。
### 压缩
压缩提交可以利用`rebase`的交互工具来完成，也可以利用这里的`reset`，使用`git reset --soft [commit]`将HEAD回退到某个版本，此时暂存区工作区的内容都未有任何改变，此时commit即可创建一个从回退到的版本到回退前版本的直接提交，从而达到压缩的目的。
***
## checkout
### 不带路径
运行 `git checkout [branch]` 与运行` git reset --hard [branch]` 非常相似，它会更新所有三棵树使其看起来像 `[branch]`，不过有两点重要的区别。
首先不同于 `reset --hard`，`checkout `对工作目录是安全的，它会通过检查来确保不会将已更改的文件吹走。其实它还更聪明一些。它会在工作目录中先试着简单合并一下，这样所有_还未修改过的_文件都会被更新。而` reset --hard` 则会不做检查就全面地替换所有东西。第二个重要的区别是如何更新 `HEAD`。`reset` 会移动 `HEAD` 分支的指向，而 `checkout` 只会移动 `HEAD` 自身来指向另一个分支。
### 带路径
运行 `checkout` 的另一种方式就是指定一个文件路径，它就像` git reset [branch] file` 那样用该次提交中的那个文件来更新索引，但是它也会覆盖工作目录中对应的文件,但不会改变HEAD指向。
***
## reset 和checkout的比较
|                        |HEAD |Index |Workdir |WD Safe?|
|:-----------------------|:-----|:-----|:-----|:-----|
|Commit Level            |
|reset --soft [commit]   | REF |NO |NO |YES|
|reset [commit]          |REF |YES |NO |YES|
|reset --hard [commit]   |REF |YES |YES |NO|
|checkout [commit]       |HEAD |YES |YES |YES|
|File Level              |
|reset (commit) [file]   |NO |YES |NO |YES|
|checkout (commit) [file]|NO |YES |YES |NO|
***
## 提交准则
1. 找出空白错误
```
$ git diff --check
```
2. 尝试让每一个提交成为一个逻辑上的独立变更集
>如果可以，尝试让改动可以理解 - 不要在整个周末编码解决五个问题，然后在周一时将它们提交为一个巨大的提交。即使在周末期间你无法提交，在周一时使用暂存区域将你的工作最少拆分为每个问题一个提交，并且为每一个提交附带一个有用的信息。如果其中一些改动修改了同一个文件，尝试使用 `git add --patch` 来部分暂存文件。
3. 提交信息
>有一个创建优质提交信息的习惯会使 Git 的使用与协作容易的多。一般情况下，信息应当以少于 50 个字符（25个汉字）的单行开始且简要地描述变更，接着是一个空白行，再接着是一个更详细的解释Git 项目要求一个更详细的解释，包括做改动的动机和它的实现与之前行为的对比 - 这是一个值得遵循的好规则。
***
## git 工具
1. 简短的 SHA-1
>你只需要提供 SHA-1 的前几个字符就可以获得对应的那次提交

之前看到教程上别人都用短的sha码，我都不知道短的码从哪里来的¯\_(ツ)_/¯，原来这个是要那个好长的码的前几位（至少4位）就行了。。。
下面命令等价（无歧义情况下）：
```
$ git show 1c002dd4b536e7479fe34593e72e6c6c1819e53b
$ git show 1c002dd4b536e7479f
$ git show 1c002d
```

2. 分支引用
指明一次提交最直接的方法是有一个指向它的分支引用。这样你就可以在任意一个 Git 命令中使用这个分支名来代替对应的提交对象或者 SHA-1 值。
例如，你想要查看一个分支的最后一次提交的对象，假设 topic1 分支指向ca82a6d ，那么以下的命令是等价的:
```
$ git show ca82a6dff817ec66f44342007202690a93763949
$ git show topic1
```
3. 引用日志
```
git reflog
```
引用日志记录了最近几个月你的 HEAD 和分支引用所指向的历史。

4. 祖先引用
祖先引用是另一种指明一个提交的方式。如果你在引用的尾部加上一个 ^， Git 会将其解析为该引用的上一个提交。可以使用 `HEAD^ `来查看上一个提交（HEAD可以看做当前分支），也就是 “HEAD 的父提交”。你也可以在` ^ `后面添加一个数字——例如 `d921970^2 `代表 “d921970 的第二父提交”。这个语法只适用于合并(merge)的提交，因为合并提交会有多个父提交。第一父提交是你合并时所在分支，而第二父提交是你所合并的分支。
另一种指明祖先提交的方法是 `~`。同样是指向第一父提交，因此 `HEAD~` 和 `HEAD^` 是等价的。而区别在于你在后面加数字的时候。`HEAD~2` 代表 “第一父提交的第一父提交”，也就是 “祖父提交” —— Git 会根据你指定的次数获取对应的第一父提交。`HEAD~3`也可以也可以写成 `HEAD^^^`，也是第一父提交的第一父提交的第一父提交。你也可以组合使用这两个语法 —— 你可以通过`HEAD~3^2` 来取得上一个引用的第二父提交（假设它是一个合并提交）（也就是第一父提交的第一父提交的第一父提交的第二父提交(#｀皿´)）。

5. 提交区间
 
- 双点
选出在一个分支中而不在另一个分支中的提交。
假如你有两个分支`master`和`experiment`， 你想要查看 `experiment` 分支中还有哪些提交尚未被合并入 `master` 分支。你可以使用 `master..experiment`来让 Git 显示这些提交。也就是 “在 experiment 分支中而不在 master 分支中的提交”。
```
git log master..experiment
```
另一个常用的场景是查看你即将推送到远端的内容：
```
$ git log origin/master..HEAD
```
- 多点
Git 允许你在任意引用前加上` ^ `字符或者 `--not `来指明你不希望提交被包含其中的分支。因此下列3个命令是等价的：
```
$ git log refA..refB
$ git log ^refA refB
$ git log refB --not refA
```
这个语法很好用，因为你可以在查询中指定超过两个的引用，这是双点语法无法实现的。比如，你想查看所有被refA 或 refB 包含的但是不被 refC 包含的提交，你可以输入下面中的任意一个命令:
```
$ git log refA refB ^refC
$ git log refA refB --not refC
```
- 三点
这个语法可以选择出被两个引用中的一个包含但又不被两者同时包含的提交。如果你想看 master 或者 experiment 中包含的但不是两者共有的提交，你可以执行:
```
$ git log master...experiment
```
可加参数 `--left-right`，它会显示每个提交到底处于哪一侧的分支。这会让输出数据更加清晰。

6. 交互式暂存
运行 `git add` 时使用 `-i `或者 `--interactive` 选项，Git 将会进入一个交互式终端模式:
```
$ git add -i
           staged     unstaged path
  1:    unchanged       +63/-4 learn.md

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```
- 暂存与取消暂存
如果在 What now> 提示符后键入 2 或 u，脚本将会提示想要暂存哪个文件:
```
What now> 2
           staged     unstaged path
  1:    unchanged       +63/-4 learn.md
Update>>
```
要暂存 TODO 与 index.html 文件，可以输入数字：
```
Update>> 1
           staged     unstaged path
* 1:    unchanged       +63/-4 learn.md
Update>>
```
每个文件前面的 * 意味着选中的文件将会被暂存。如果在 Update>> 提示符后不输入任何东西并直接按回车，Git 将会暂存之前选择的文件:
```
updated 1 path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:       +63/-4      nothing learn.md

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```
现在可以看到文件已被暂存，如果这时想要取消暂存TODO 文件，使用 3 或 r（撤消）选项：
```
*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 3
           staged     unstaged path
  1:       +63/-4      nothing learn.md
Revert>> 1
           staged     unstaged path
* 1:       +63/-4      nothing learn.md
Revert>>
reverted 1 path
```
再次查看 Git 状态，可以看到已经取消暂存:
```
*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:    unchanged       +63/-4 learn.md
```
如果想要查看已暂存内容的区别，可以使用 6 或 d（区别）命令。它会显示暂存文件的一个列表，可以从中选择想要查看的暂存区别。这跟你在命令行指定 git diff --cached 非常相似：
```
*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:       +63/-4      nothing learn.md

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 6
           staged     unstaged path
  1:       +63/-4      nothing learn.md
Review diff>> 1
diff --git a/learn.md b/learn.md
index acfbb81..bdf46ad 100644
--- a/learn.md
+++ b/learn.md
@@ -259,13 +259,14 @@ $ git push [remote name] :refs/tags/[tag name]
 删除远程仓库标签
 ***
...
 ----------
 ```
7. 暂存补丁
Git 也可以暂存文件的特定部分。例如，如果在 `learn.md` 文件中做了几处修改，但只想要暂存其中的一个而不是另一个，Git 会帮你轻松地完成。从交互式提示符中，输入 `5` 或 `p`（补丁）。Git 会询问你想要部分暂存哪些文件；然后，对已选择文件的每一个部分，它都会一个个地显示文件区别并询问你是否想要暂存它们：
```
*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:    unchanged       +63/-4 learn.md

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 5
           staged     unstaged path
  1:    unchanged       +63/-4 learn.md
Patch update>> 1
           staged     unstaged path
* 1:    unchanged       +63/-4 learn.md
Patch update>>
diff --git a/learn.md b/learn.md
index acfbb81..bdf46ad 100644
--- a/learn.md
+++ b/learn.md
@@ -259,13 +259,14 @@ $ git push [remote name] :refs/tags/[tag name]
 删除远程仓库标签
 ***
...
 ----------
 some change...
Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]?
```
这时有很多选项。输入 ? 显示所有可以使用的命令列表：
```
Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]? ?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```
也可以不必在交互式添加模式中做部分文件暂存 - 可以在命令行中使用 git add -p 或 git add --patch 来启动同样的脚本。
8. 储藏与清理
有时，当你在项目的一部分上已经工作一段时间后，所有东西都进入了混乱的状态，而这时你想要切换到另一个分支做一点别的事情。问题是，你不想仅仅因为过会儿回到这一点而为做了一半的工作创建一次提交。针对这个问题的答案是 `git stash save` 或`git stash`命令。储藏会处理工作目录的脏的状态 - 即，修改的跟踪文件与暂存改动 - 然后将未完成的修改保存到一个栈上，而你可以在任何时候重新应用这些改动。`git stash save`可以直接跟注释信息。
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   main.c

no changes added to commit (use "git add" and/or "git commit -a")

$ git stash
Saved working directory and index state WIP on master: 163617c Creat the first file main.c

$ git status
On branch master
nothing to commit, working tree clean

$ git stash list
stash@{0}: WIP on master: 163617c Creat the first file main.c
```
通过git stash apply将你刚刚储藏的工作重新应用:
```
$ git stash apply
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   main.c

no changes added to commit (use "git add" and/or "git commit -a")

```
如果想要应用其中一个更旧的储藏，可以通过名字指定它，像这样：`git stash apply stash@{2}`。如果不指定一个储藏，Git 认为指定的是最近的储藏。
本次应用储藏时有一个干净的工作目录，并且尝试将它应用在保存它时所在的分支；但是有一个干净的工作目录与应用在同一分支并不是成功应用储藏的充分必要条件。可以在一个分支上保存一个储藏，切换到另一个分支，然后尝试重新应用这些修改。当应用储藏时工作目录中也可以有修改与未提交的文件 - 如果有任何东西不能干净地应用，Git 会产生合并冲突。`git stash apply`只能恢复工作目录，如果想把暂存区也按照贮藏时的暂存区恢复的话，可以加上`--index`。
应用选项只会尝试应用暂存的工作 - 在堆栈上还有它。可以运行` git stash drop` 加上将要移除的储藏的名字来移除它,也可以运行` git stash pop` 来应用储藏然后立即从栈上扔掉它。`git satsh clear`可以删除所有stash。
创造性储藏
- 有几个储藏的变种可能也很有用。第一个非常流行的选项是 `stash save` 命令的 `--keep-index` 选项。它告诉Git 不要储藏任何你通过 `git add` 命令已暂存的东西。当你做了几个改动并只想提交其中的一部分，过一会儿再回来处理剩余改动时，这个功能会很有用。
- 另一个经常使用储藏来做的事情是像储藏跟踪文件一样储藏未跟踪文件。默认情况下，`git stash` 只会储藏已经在索引中的文件。如果指定 `--include-untracked` 或 `-u `标记，Git 也会储藏任何创建的未跟踪文件。
- 最终，如果指定了 `--patch` 标记，Git 不会储藏所有修改过的任何东西，但是会交互式地提示哪些改动想要储藏、哪些改动需要保存在工作目录中。
从储藏创建一个分支
如果储藏了一些工作，将它留在那儿了一会儿，然后继续在储藏的分支上工作，在重新应用工作时可能会有问题。如果应用尝试修改刚刚修改的文件，你会得到一个合并冲突并不得不解决它。如果想要一个轻松的方式来再次测试储藏的改动，可以运行 `git stash branch` 创建一个新分支，检出储藏工作时所在的提交，重新在那应用工作，然后在应用成功后扔掉储藏。
9. 清理工作目录
```
git clean
```
它被设计为从工作目录中移除未被追踪的文件，你需要谨慎地使用这个命令，因为移除的文件很可能找不回来了。一个更安全的选项是运行 git stash --all 来移除每一样东西并存放在栈中。你可以使用`git clean`命令去除冗余文件或者清理工作目录。使用`git clean -f -d`命令来移除工作目录中所有未追踪的文件以及空的子目录。-f 意味着 强制 或 “确定移除”。如果只是想要看看它会做什么，可以使用 -n 选项来运行命令，这意味着 “做一次演习然后告诉你 将要 移除什么”。
默认情况下，git clean 命令只会移除没有忽略的未跟踪文件。任何与 .gitiignore 或其他忽略文件中的模式匹配的文件都不会被移除。如果你也想要移除那些文件，例如为了做一次完全干净的构建而移除所有由构建生成的 .o 文件，可以给 `clean` 命令增加一个 `-x` 选项。加`-i`参数可以以交互模式运行`clean`命令。

10. 搜索
- Git Grep
Git 提供了一个 grep 命令，你可以很方便地从提交历史或者工作目录中查找一个字符串或者正则表达式。
可以传入 `-n` 参数来输出 Git 所找到的匹配行行号。
使用 `--count `选项来使 Git 输出概述的信息，仅仅包括哪些文件包含匹配以及每个文件包含了多少个匹配。
如果你想看匹配的行是属于哪一个方法或者函数，你可以传入 `-p` 选项.
 使用`--break` 和 `--heading` 选项可以使输出更加容易阅读。

11. Git日志搜索
或许你不想知道某一项在 哪里 ，而是想知道是什么 时候 存在或者引入的。`git log` 命令有许多强大的工具可以通过提交信息甚至是 diff 的内容来找到某个特定的提交。
使用` -S `选项来显示新增和删除字符串的提交。
使用 `-G `选项来使用正则表达式搜索。
使用 `-L `选项展示代码中一行或者一个函数的历史。
12. 子模块
子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。
