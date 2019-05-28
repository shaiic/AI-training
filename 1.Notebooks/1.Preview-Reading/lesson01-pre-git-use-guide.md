# Git的使用技巧及总结

> 高冲 2019年5月28日
>
> 主要参考作者撰写的博客：[Git使用总结](http://blog.a-stack.com/2017/02/02/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0-Pro-Git/)

## 写在前面

​	本篇总结内容主要整理自《Pro Git》，该书由GitHub员工Scott Chacon和另一位爱好者Ben Straub共同编写，主要介绍了Git使用基础和原理，适合Git爱好者和初学者参考。
> The entire Pro Git book, written by Scott Chacon and Ben Straub and published by Apress, is available here. All content is licensed under the [Creative Commons Attribution Non Commercial Share Alike 3.0 license](https://creativecommons.org/licenses/by-nc-sa/3.0/). Print versions of the book are available on [Amazon.com](http://www.amazon.com/Pro-Git-Scott-Chacon/dp/1484200772?ie=UTF8&camp=1789&creative=9325&creativeASIN=1430218339&linkCode=as2&tag=git-sfconservancy-20).

可通过如下地址获取（另外本文末尾提供了不同格式下载的地址）：

- 官网：[http://git-scm.com/book/en/v2](http://git-scm.com/book/en/v2)（第二版）
- 中文翻译：[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)（第二版）
  ​

### 同类工具
- Subversion 
- Perforce
- Bazaar
- Git特点
  - 数据存储方式---》`a stream of snapshots`
  - 离线处理和编辑
  - Checksum 算法：SHA-1 hash

### 0. 环境配置

> Git全局配置文件的改写通过添加`--global`实现
> Git的配置文件（此处仅针对某个repo）位置为`.git/config`

1. **基本信息配置**
   ```bash
   $ git config --global user.name "John Doe"
   $ git config --global user.email johndoe@example.com
   $ git config --global pull.rebase true
   $ git config --global core.editor emacs
   $ git config --global color.ui true
   # Windows环境下，编辑器的配置如下
   $ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst
   ```
   配置完成之后，可以通过如下命令检查配置情况：
   ```bash
   $ git config --list
   ```
2. 别名设置

   ```basic
   $ git config --global alias.unstage 'reset HEAD --'
   $ git config --global alias.last 'log -1 HEAD'
   ...
   ```
### 1. Git基础
​	下图描述了git处理文件的生命周期
![lifecycleForGit](images/lifecycleForGit.jpg)

#### 关于`.gitignore`文件
​	文件 `.gitignore` 的格式规范如下：
- 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以`/`开头防止递归。
- 匹配模式可以以`/`结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号`!`取反。
> 所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号`*`匹配零个或多个任意字符；`[abc]` 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号`?`只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如` [0-9] `表示匹配所有 0 到 9 的数字）。 使用两个星号`**` 表示匹配任意中间目录，比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。更多内容请参考：https://github.com/github/gitignore

​	举个🌰[栗子]：
```bash
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```
#### git rm

​	我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 `.a `这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用` —cached` 选项：
```basic
$ git rm --cached README
```
​	`git rm`命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：
```basic
$ git rm log/*.log
```
​	注意到星号 `*` 之前的反斜杠` \`， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 `log/` 目录下扩展名为 `.log` 的所有文件。 类似的比如：
```basic
$ git rm *~
```
​	该命令为删除以 ~ 结尾的所有文件。

#### git log

​	git log 有许多选项可以帮助你搜寻你所要找的提交。
1. 一个常用的选项是` -p`，用来显示每次提交的内容差异。 你也可以加上` -2` 来仅显示最近两次提交：
```basic
$ git log -p -2
```
2. 如果你想看到每次提交的简略的统计信息，你可以使用 `—stat` 选项：

```bash
EbbyMandeMBP:blog 520XM$ git log --stat
commit 1a158fd41bece4caed80210526e8700dae73a527
Author: ddebby <ebby.dd@gmail.com>
Date:   Thu Feb 2 19:02:31 2017 +0800

    新增两篇读书笔记
    
    Signed-off-by: ddebby <ebby.dd@gmail.com>

 ...344\271\246\347\254\224\350\256\260-Pro-Git.md" |  79 +++++++++++++++++++++
 ...56\260-\350\205\276\350\256\257\344\274\240.md" |  65 +++++++++++++++++
 source/images/blog/Pro Git Version 2.jpg           | Bin 0 -> 134581 bytes
 source/images/blog/pro-git-cover.jpeg              | Bin 0 -> 21289 bytes
 ...56\257\344\274\240\345\260\201\351\235\242.jpg" | Bin 0 -> 12898 bytes
 ...0\206-\350\205\276\350\256\257\344\274\240.PNG" | Bin 0 -> 24520 bytes
 themes/next/source/css/_custom/custom.styl         |   3 +-
 themes/next/source/css/_variables/custom.styl      |  12 ++--
 8 files changed, 152 insertions(+), 7 deletions(-)
```

3. 还可以给出若干搜索条件，列出符合的提交。 用 `--author` 选项显示指定作者的提交，用 `--grep` 选项搜索提交说明中的关键字。 （请注意，如果要得到同时满足这两个选项搜索条件的提交，就必须用 `--all-match `选项。否则，满足任意一个条件的提交都会被匹配出来）
4. 另一个非常有用的筛选选项是 `-S`，可以列出那些添加或移除了某些字符串的提交。 比如说，你想找出添加或移除了某一个特定函数的引用的提交，你可以这样使用：

```bash
$ git log -S function_name
```
5. 最后一个很实用的 git log 选项是路径（path）， 如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。 因为是放在最后位置上的选项，所以用两个短划线（--）隔开之前的选项和后面限定的路径名。

| **选项**            | **说明**            |
| ----------------- | ----------------- |
| -(n)              | 仅显示最近的 n 条提交      |
| --since, --after  | 仅显示指定时间之后的提交。     |
| --until, --before | 仅显示指定时间之前的提交。     |
| --author          | 仅显示指定作者相关的提交。     |
| --committer       | 仅显示指定提交者相关的提交。    |
| --grep            | 仅显示含指定关键字的提交      |
| -S                | 仅显示添加或移除了某个关键字的提交 |

#### git diff

 Git 提供了一种比较便捷的方式：三点语法。 对于 `diff` 命令来说，你可以通过把 `...` 置于另一个分支名后来对该分支的最新提交与两个分支的共同祖先进行比较：

```
$ git diff master...contrib
```

该命令仅会显示自当前特性分支与 master 分支的共同祖先起，该分支中的工作。 这个语法很有用，应该牢记。

#### 远程仓库

**添加远程仓库**
​	我在之前的章节中已经提到并展示了如何添加远程仓库的示例，不过这里将告诉你如何明确地做到这一点。 运行 `git remote add <shortname> <url>` 添加一个新的远程 Git 仓库。
**从远程仓库中抓取与拉取**
​	如果你想拉取远程仓库中有但你没有的信息，可以运行：
```basic
$ git fetch [remote-name]
```
​	必须注意 git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

#### 标签

**附注标签**
在 Git 中创建一个附注标签是很简单的。 最简单的方式是当你在运行 tag 命令时指定` -a` 选项：
```basic
$ git tag -a v1.4 -m 'my version 1.4'

$ git tag
v0.1
v1.3
v1.4
```
​	`-m `选项指定了一条将会存储在标签中的信息。 如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。

**后期打标签**
​	要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）:
```basic
$ git tag -a v1.2 9fceb02
```
**共享标签**
​	默认情况下，git push 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样 - 你可以运行 
```basic
$ git push origin [tagname] 
# or
$ git push origin --tags
```
**检出标签**
​	在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 `git checkout -b [branchname][tagname] `在特定的标签上创建一个新分支：
```basic
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
#### 分支管理 `git branch`
​	如果需要查看每一个分支的最后一次提交，可以运行 `git branch -v `命令：
```basic
$ git branch -v
* dev    1a158fd 新增两篇读书笔记
  master f8f05fb Accept Merge Request #1 Merge Resource branch to Master branch : (resource -> master)
```
​	`—merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。 如果要查看哪些分支已经合并到当前分支，可以运行：
```basic
$ git branch --merged
  iss53
* master
```
​	因为之前已经合并了 iss53 分支，所以现在看到它在列表中。 在这个列表中分支名字前没有 * 号的分支通常可以使用 git branch -d 删除掉；你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。
​	查看所有包含未合并工作的分支，可以运行：
```basic
$ git branch --no-merged
  testing
```
​	这里显示了其他分支。
> 未被合并的分支默认是无法被删除，不过可以通过`git branch -D <branch_name>`来强制删除。

- 拉取远程分支：
```basic
$ git checkout -b [branch] [remotename]/[branch]
```
- 如果想要查看设置的所有跟踪分支，可以使用 `git branch` 的 `-vv` 选项。 这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。
```basic
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```
- 删除远程服务器的分支
```basic
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```
- `git merge`与`git rebase`
> 总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样，你才能享受到两种方式带来的便利。详情参考：https://git-scm.com/book/zh/v2/Git-分支-变基

- 将当前分支推送为远程新分支

```bash
git push origin local_branch:remote_branch
```

#### git stash

**储藏与清理**

在一个分支上做了一些修改，在不想commit的情况下，切换分支，需要将修改的内容存储起来，使用如下命令：

```bash
git stash | git stash save
git stash list
git stash apply
# 移除某个stach内容
git stach drop stach@{0}
```

### 2. Git 服务器

- **生成裸仓库**
  ​在开始架设 Git 服务器前，需要把现有仓库导出为裸仓库——即一个不包含当前工作目录的仓库。 这通常是很简单的。 为了通过克隆你的仓库来创建一个新的裸仓库，你需要在克隆命令后加上 `--bare`选项 按照惯例，裸仓库目录名以 `.git` 结尾，就像这样：
```basic
$ git clone --bare my_project my_project.git
Cloning into bare repository 'my_project.git'...
done.
```
现在，你的 my_project.git 目录中应该有 Git 目录的副本了。
- **把裸仓库放到服务器上**
  只需要将上述`my_project.git `复制到某个目录下即可通过如下命令访问：
```basic
$ git clone user@git.example.com:/opt/git/my_project.git
```
​	如果到该项目目录中运行 `git init` 命令，并加上 `--shared `选项，那么 Git 会自动修改该仓库目录的组权限为可写。
```basic
$ ssh user@git.example.com
$ cd /opt/git/my_project.git
$ git init --bare --shared
```
> 基于以上简单配置的简单权限管理策略：
> 1. 主机上建立一个 *git* 账户，让每个需要写权限的人发送一个 SSH 公钥，然后将其加入 git 账户的 ~/.ssh/authorized_keys 文件。 这样一来，所有人都将通过 *git* 账户访问主机。 这一点也不会影响提交的数据——访问主机用的身份不会影响提交对象的提交者信息。
> 2. 另一个办法是让 SSH 服务器通过某个 LDAP 服务，或者其他已经设定好的集中授权机制，来进行授权。 只要每个用户可以获得主机的 shell 访问权限，任何 SSH 授权机制你都可视为是有效的。

#### 维护项目

**使用`format-patch`生成补丁**

- 适用于邮件通知项目管理者合并补丁内容

**使用 `apply` 命令应用补丁**

​	如果你收到了一个使用 `git diff` 或 Unix `diff` 命令（不推荐使用这种方式，具体见下一节）创建的补丁，可以使用 `git apply` 命令来应用。 假设你将补丁保存在了 `/tmp/patch-ruby-client.patch` 中，可以这样应用补丁：

```
$ git apply /tmp/patch-ruby-client.patch
```

这会修改工作目录中的文件。 它与运行 `patch -p1` 命令来应用补丁几乎是等效的，但是这种方式更加严格，相对于 patch 来说，它能够接受的模糊匹配更少。

在实际应用补丁前，你还可以使用 `git apply` 来检查补丁是否可以顺利应用——即对补丁运行 `git apply --check` 命令：

```
$ git apply --check 0001-seeing-if-this-helps-the-gem.patch
error: patch failed: ticgit.gemspec:1
error: ticgit.gemspec: patch does not apply
```

如果没有产生输出，则该补丁可以顺利应用。 如果检查失败了，该命令还会以一个非零的状态退出，所以需要时你也可以在脚本中使用它。

##### 准备一次发布

现在你可以发布一个构建了。 其中一件事情就是为那些不使用 Git 的可怜包们创建一个最新的快照归档。 使用 `git archive` 命令完成此工作：

```
$ git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
$ ls *.tar.gz
v1.6.2-rc1-20-g8c5b85c.tar.gz
```

如果有人将这个压缩包解压，他就可以得到你的项目文件夹的最新快照。 你也可以以类似的方式创建一个 zip 压缩包，但此时你应该向 `git archive` 命令传递 `--format=zip` 选项：

```
$ git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

现在你有了本次发布的一个 tar 包和一个 zip 包，可以将其上传到网站或以电子邮件的形式发送给人们。

##### 制作提交简报

现在是时候通知邮件列表里那些好奇你的项目发生了什么的人了。 使用 `git shortlog` 命令可以快速生成一份包含从上次发布之后项目新增内容的修改日志（changelog）类文档。 它会对你给定范围内的所有提交进行总结；比如，你的上一次发布名称是 v1.0.1，那么下面的命令可以给出上次发布以来所有提交的总结：

```
$ git shortlog --no-merges master --not v1.0.1
Chris Wanstrath (8):
      Add support for annotated tags to Grit::Tag
      Add packed-refs annotated tag support.
      Add Grit::Commit#to_patch
      Update version and History.txt
      Remove stray `puts`
      Make ls_tree ignore nils

Tom Preston-Werner (4):
      fix dates in history
      dynamic version method
      Version bump to 1.0.2
      Regenerated gemspec for version 1.0.2
```

这份整洁的总结包括了自 v1.0.1 以来的所有提交，并且已经按照作者分好组，你可以通过电子邮件将其直接发送到列表中。

## Git工具

### 双点

最常用的指明提交区间语法是双点。 这种语法可以让 Git 选出在一个分支中而不在另一个分支中的提交。你想要查看 experiment 分支中还有哪些提交尚未被合并入 master 分支。 你可以使用 `master..experiment` 来让 Git 显示这些提交。也就是 “在 experiment 分支中而不在 master 分支中的提交”：

```
$ git log master..experiment
```

反过来，如果你想查看在 `master` 分支中而不在 `experiment` 分支中的提交，你只要交换分支名即可。`experiment..master` 会显示在 `master` 分支中而不在 `experiment` 分支中的提交：

```
$ git log experiment..master
```

这可以让你保持 `experiment` 分支跟随最新的进度以及查看你即将合并的内容。 另一个常用的场景是查看你即将推送到远端的内容：

```
$ git log origin/master..HEAD
```

这个命令会输出在你当前分支中而不在远程 `origin` 中的提交。 如果你执行了 `git push` 并且你的当前分支正在跟踪 `origin/master`，`git log origin/master..HEAD` 所输出的提交将会被传输到远端服务器。 如果你留空了其中的一边， Git 会默认为 HEAD。 例如， `git log origin/master..` 将会输出与之前例子相同的结果 —— Git 使用 HEAD 来代替留空的一边。

### 多点

双点语法很好用，但有时候你可能需要两个以上的分支才能确定你所需要的修订，比如查看哪些提交是被包含在某些分支中的一个，但是不在你当前的分支上。 Git 允许你在任意引用前加上 `^` 字符或者 `--not`来指明你不希望提交被包含其中的分支。 因此下列3个命令是等价的：

```
$ git log refA..refB
$ git log ^refA refB
$ git log refB --not refA
```

这个语法很好用，因为你可以在查询中指定超过两个的引用，这是双点语法无法实现的。 比如，你想查看所有被 `refA` 或 `refB` 包含的但是不被 `refC` 包含的提交，你可以输入下面中的任意一个命令

```
$ git log refA refB ^refC
$ git log refA refB --not refC
```

这就构成了一个十分强大的修订查询系统，你可以通过它来查看你的分支里包含了哪些东西。

### 搜索

例如，如果我们想找到 `ZLIB_BUF_MAX` 常量是什么时候引入的，我们可以使用 `-S` 选项来显示新增和删除该字符串的提交。

```
$ git log -SZLIB_BUF_MAX --oneline
e01503b zlib: allow feeding more than 4GB in one go
ef49a7a zlib: zlib can only process 4GB at a time
```

如果我们查看这些提交的 diff，我们可以看到在 `ef49a7a` 这个提交引入了常量，并且在 `e01503b` 这个提交中被修改了。

### 行日志搜索

行日志搜索是另一个相当高级并且有用的日志搜索功能。 这是一个最近新增的不太知名的功能，但却是十分有用。 在 `git log` 后加上 `-L` 选项即可调用，它可以展示代码中一行或者一个函数的历史。

例如，假设我们想查看 `zlib.c` 文件中`git_deflate_bound` 函数的每一次变更，我们可以执行 `git log -L :git_deflate_bound:zlib.c`。 Git 会尝试找出这个函数的范围，然后查找历史记录，并且显示从函数创建之后一系列变更对应的补丁。

### 压缩提交

以下命令将HEAD调回到两次提交之前，做一次压缩提交。更多内容详见：[Pro Git中的说明](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)

```bash
git reset --soft HEAD~2
```

### 还原提交

如果移动分支指针并不适合你，Git 给你一个生成一个新提交的选项，提交将会撤消一个已存在提交的所有修改。 Git 称这个操作为 “还原”，在这个特定的场景下，你可以像这样调用它：

```
$ git revert -m 1 HEAD
[master b1d8379] Revert "Merge branch 'topic'"
```

`-m 1` 标记指出 “mainline” 需要被保留下来的父结点。 当你引入一个合并到 `HEAD`（`git merge topic`），新提交有两个父结点：第一个是 `HEAD`（`C6`），第二个是将要合并入分支的最新提交（`C4`）。 在本例中，我们想要撤消所有由父结点 #2（`C4`）合并引入的修改，同时保留从父结点 #1（`C4`）开始的所有内容。

### 撤销提交

如果这个不想要的合并提交只存在于你的本地仓库中，最简单且最好的解决方案是移动分支到你想要它指向的地方。 大多数情况下，如果你在错误的 `git merge` 后运行 `git reset --hard HEAD~`，这会重置分支指向。

这个方法的缺点是它会重写历史，在一个共享的仓库中这会造成问题的。 查阅 [变基的风险](https://git-scm.com/book/zh/v2/ch00/_rebase_peril) 来了解更多可能发生的事情；用简单的话说就是如果其他人已经有你将要重写的提交，你应当避免使用 `reset`。 如果有任何其他提交在合并之后创建了，那么这个方法也会无效；移动引用实际上会丢失那些改动。

## GitHub的配置及基本测试

1. ssh key的创建

```bash
 $ ssh-keygen -t rsa -C "your_email@youremail.com"
```

2. 上传`id_rsa.pub` 到github配置
3. 测试

```bash
$ ssh -T git@github.com
```

正确的返回结果

```bash
gaoc@DataScience:/data2/ml-cousera$ ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Hi ddebby! You've successfully authenticated, but GitHub does not provide shell access.
```

4. 添加origin源

```bash
$ git remote add origin git@github.com:yourName/yourRepo.git
```

> 不要直接使用https的链接地址，还需要不停的输入用户名/密码验证



## 获取不同版本的电子书

- [PDF格式](https://progit2.s3.amazonaws.com/en/2014-10-28-26e19/progit-en.54.pdf)
- [Epub格式](https://progit2.s3.amazonaws.com/en/2014-10-28-26e19/progit-en.54.epub)
- [Mobi格式](https://progit2.s3.amazonaws.com/en/2014-10-28-26e19/progit-en.54.mobi)
- [HTML在线阅读](https://progit2.s3.amazonaws.com/en/2014-10-28-26e19/progit-en.54.zip)
