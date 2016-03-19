title: git 

## git usage
[TOC]

## .gitignore, ~/.gitignore_global
man gitignore 

## ~/.gitconfig
git config --global --get user.name
git config --list
git config -l
git config --global -l
git config --global -e
git config --global --add user.name "Yu Guangzhen"
git config --global --replace user.name "andy"
git config --global core.editor "vi"   // 设置git编辑器

## 设置Git显示语言
1. 用`LANG=en_GB git`替换`git`即可
    ```
    LANG=en_GB git commit
    ```
2. 可以将其设置到bash配置中：
    ```
    echo "alias git='LANG=en_GB git'" >> ~/.bashrc
    ```

## Configure your personal ignore file
http://www.programblings.com/2008/10/22/git-global-ignores/
I like to stick to conventions so I call my file .gitignore_global, and I put it in my home directory. But that’s up to you, really.
```
git config --global core.excludesfile ~/.gitignore_global
```
Note that there’s one little gotcha to be aware of. If you prefer to edit the .gitconfig file directly (or if you use a weird shell), git expects an absolute path. In the example above, bash converted the ~ shorthand to my home directory.
Now I just add ignore globs to it like any other project level (directory level, really) git ignore file.
```
echo .DS_Store >> ~/.gitignore
```
Once I’ve ignored all my favorite useless files, I can get cracking and never worry about them again.

## 初始化空git repository
git init

## 查看状态
git status

## 添加文件到git index
git add . // 添加当前目录下的所有文件和子目录

## 强制添加到git，对ignore的文件使用
git add -f ignorefile

## 仅删git index记录，不删文件
git rm -r --cached directory
git rm --cached file

## 删git index记录，且删文件
git rm -rf directory

## 提交
git commit -m "first commit"

## 查询log
git log -n // 查看n条记录
git log --pretty=oneline // 仅显示一行
git log -p // 显示变化内容
git log --name-status // 可以查看到那些文件有那个变动（A,M,...)
git log --stat // 可以查看到那些文件被影响
git show // 查看最近一次的变更内容

## 查看改变
git diff 版本号1 版本号2

## 克隆repository
git clone repository [directory]

## 查看远程分支
git remote -v
git remote show origin
git remote add origin ~/Desktop/ // 添加远程分支名
git remote set-url origin ~/workspace/Android/andy_android/ // 重新设置远程分支url

## 拉取远程分支内容到本地
git pull

## 推送本地修改到远程
git push // 此命令遇到non-bare repository时，将出如下错误，解决方法见下节内容
```
Andy:andy_android YuGuangzhen$ git push
Counting objects: 7, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 602 bytes | 0 bytes/s, done.
Total 4 (delta 3), reused 0 (delta 0)
remote: error: refusing to update checked out branch: refs/heads/master
remote: error: By default, updating the current branch in a non-bare repository
remote: error: is denied, because it will make the index and work tree inconsistent
remote: error: with what you pushed, and will require 'git reset --hard' to match
remote: error: the work tree to HEAD.
remote: error: 
remote: error: You can set 'receive.denyCurrentBranch' configuration variable to
remote: error: 'ignore' or 'warn' in the remote repository to allow pushing into
remote: error: its current branch; however, this is not recommended unless you
remote: error: arranged to update its work tree to match what you pushed in some
remote: error: other way.
remote: error: 
remote: error: To squelch this message and still keep the default behaviour, set
remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To /Users/YuGuangzhen/workspace/Android/andy_android/
 ! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to '/Users/YuGuangzhen/workspace/Android/andy_android/'

```

## 设置允许被push
git config receive.denyCurrentBranch ignore // 如此设置后，non-bare repository也可以被push，不推荐这样设置  此命令实际修改的是./.git/config文件

<!-- ??如果希望保留生产服务器上所做的改动,仅仅并入新配置项, 处理方法如下:
http://www.cnlvzi.com/index.php/Index/article/id/119

git stash

git pull

git stash pop

然后可以使用git diff -w +文件名 来确认代码自动合并的情况.


反过来,如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:

git reset --hard

git pull 


http://www.01happy.com/git-resolve-conflicts/

有时候使用Git工作得小心，特别是涉及到一些高级操作，甚至一些很小的操作，例如删除一个分支。其实没有必要，只要git库不删除，就可以恢复，因为git的历史记录是不可修改的，也就是说你不能更改任何已经发生的事情。你做的任何操作都只是在原来的操作上修改。也就是说，即使你删除了一个分支，修改了一个提交，或者强制重置，你仍然可以回滚这些操作。

工具/原料
git
方法/步骤
1
git reflog。reflog它会记录所有HEAD的历史，也就是说当你做 reset，checkout等操作的时候，这些操作会被记录在reflog中。
git reset -hard 的误操作的解决办法
2
如果我们要找回我们第二次commit，只需要做如下操作：
git reset --hard 98abc5a
3
所以，如果因为reset等操作丢失一个提交的时候，你总是可以把它找回来。



-->

## git utf-8

1. 使用git add添加要提交的文件的时候，如果文件名是中文，会显示形如274\232\350\256\256\346\200\273\347\273\223.png的乱码
```
git config --global core.quotepath false

```
2 .在MsysGit中，使用git log显示提交的中文log乱码
解决方法：
    1. 设置git gui的界面编码
    ```
    git config --global gui.encoding utf-8
    ```
    2. 设置 commit log 提交时使用 utf-8 编码，可避免服务器上乱码，同时与linux上的提交保持一致！
    ```
    git config --global i18n.commitencoding utf-8
    ```
    3. 使得在 $ git log 时将 utf-8 编码转换成 gbk 编码，解决Msys bash中git log 乱码。
    ```
    git config --global i18n.logoutputencoding gbk
    ```
    4. 使得 git log 可以正常显示中文（配合i18n.logoutputencoding = gbk)，在 /etc/profile 中添加：
    ``
    export LESSCHARSET=utf-8
    ```
3. 在MsysGit自带的bash中，使用ls命令查看中文文件名乱码。cygwin没有这个问题。
解决方案：
使用 ls --show-control-chars 命令来强制使用控制台字符编码显示文件名，即可查看中文文件名。
为了方便使用，可以编辑 /etc/git-completion.bash（在git安装目录下），新增一行 alias ls="ls --show-control-chars"

## MsysGit相关设置
1. 全局的.gitconfig 在`~`目录下面，可以进入git bash后，通过如下方法取得
```
cd ~
pwd
```
2. 配置全局.gitignore文件
```
git config --global core.excludesfile ~/.gitignore
```
3. 在MsysGit自带的bash中，使用ls命令查看中文文件名乱码
在git安装目录的/etc/git-completion.bash末尾加入下面代码
```
alias ls="ls --show-control-chars"
```

## git忽略mode的配置(old mode 100755 new mode 100644 让git忽略文件权限检查)
```
git config --global core.filemode false  // 当遇到设置此值不生效时，有可能是.git/config中把filemode设成了true，因此全局设置被覆盖
git config --add core.filemode false // 设置此值将覆盖全局设置
```

## [github 每次都输入密码](http://blog.csdn.net/yuquan0821/article/details/8210944)
在github.com上 建立了一个小项目，可是在每次push  的时候，都要输入用户名和密码，很是麻烦

原因是使用了https方式 push

在termail里边 输入  git remote -v 

可以看到形如一下的返回结果

origin https://github.com/yuquan0821/demo.git (fetch)

origin https://github.com/yuquan0821/demo.git (push)

下面把它换成ssh方式的。

1. git remote rm origin
2. git remote add origin git@github.com:yuquan0821/demo.git
3. git push origin 
版权声明：本文为博主原创文章，未经博主允许不得转载。


## [github ssh 配置](http://stackoverflow.com/questions/1521496/new-to-git-git-push-origin-master-ssh-exchange-identifiction-connection-clo)
1. [设置代理vi ~/.ssh/config](http://stackoverflow.com/questions/19161960/connect-with-ssh-through-a-proxy)
```
Host github github.com
Hostname github.com 
User git
ProxyCommand      nc -X connect -x web-proxy.oa.com:8080 %h %p
```

2. GitHub is highly secured and follow ssh-rsa So we need to setup as ssh public key for our connection, and let github know about it.
take terminal and as user ( not root, usually many of us have a habit of typing sudo su as the first commang at terminal, this time avoid it) type
```
ssh-keygen -t rsa -C "yourmailid@gmail.com"
```
Here, -t -> tells which encryption -C ->try to use the same mail id you have given ti github (for ease of memory)
now you will get two files id_rsa and id_rsa.pub in ~/.ssh/
now copy the whole content in file id_rsa.pub without altering the content of the file.
Now go back to you github account. go to account settings >>> SSH Public Keys Add a new Key and paste the content you copied into field "key" and save (give a title of your choice).
now github know to process the requests from your system.
now try
```
$ssh git@github.com
```
The SSH key on your machine doesn't match the one that you have on record with GitHub. Type
```
cat ~/.ssh/id_rsa.pub | pbcopy
```
which will copy your public key to the clibboard. Then go to GitHub account settings and add it as a new key.


## [回滚 git reset/revert 回退回滚取消提交返回上一版本](http://yijiebuyi.com/blog/8f985d539566d0bf3b804df6be4e0c90.html)

大致分为下面2种情况:
### 1. 没有push —— `git reset [ --soft | --mixed | --hard ] <commit...>`
这种情况发生在你的本地代码仓库,可能你add ,commit 以后发现代码有点问题,准备取消提交,用到下面命令
```
git reset c011eb3c20ba6fb38cc94fe5a8dda366a3990c61
```
选项说明：
+ --mixed(默认值）: 会保留源码,只是将git commit和index 信息回退到了某个版本. `git reset --mixed`等价于`git reset`
+ --soft: 保留源码,只回退到commit 信息到某个版本.不涉及index的回退,如果还需要提交,直接commit即可.
+ --hard: 源码也会回退到某个版本,commit和index 都回回退到某个版本.(注意,这种方式是改变本地代码仓库源码)
备注：如果已经push，也使用 git reset --hard <commit...> 回退代码到某个版本之前，那么当把本地代码修改完提交的时候你会发现全是冲突……这个时候要用revert就没问题

### 2. 已经push —— `git revert <commit...>`
对于已经把代码push到线上仓库,你回退本地代码其实也想同时回退线上代码,回滚到某个指定的版本,线上,线下代码保持一致.你要用到下面的命令
git revert用于反转提交,执行evert命令时要求工作树必须是干净的.
revert 之后你的本地代码会回滚到指定的历史版本,这时你再 git push 既可以把线上的代码更新.(这里不会像reset造成冲突的问题)
revert 使用,需要先找到你想回滚版本唯一的commit标识代码,可以用 git log 或者在adgit搭建的web环境历史提交记录里查看.
```
git revert c011eb3c20ba6fb38cc94fe5a8dda366a3990c61
```
通常,前几位即可
```
git revert c011eb3
```
__git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit__

### 两者区别看似达到的效果是一样的,其实完全不同:
1. 上面我们说的如果你已经push到线上代码库, reset 删除指定commit以后,你git push可能导致一大堆冲突.但是revert 并不会.
2. 如果在日后现有分支和历史分支需要合并的时候,reset 恢复部分的代码依然会出现在历史分支里.但是revert 方向提交的commit 并不会出现在历史分支里.
3. reset 是在正常的commit历史中,删除了指定的commit,这时 HEAD 是向后移动了,而 revert 是在正常的commit历史中再commit一次,只不过是反向提交,他的 HEAD 是一直向前的.



