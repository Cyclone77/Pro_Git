<!-- TOC -->

- [Pro_Git](#pro_git)
- [Git 初次配置](#git-初次配置)
- [Git基础](#git基础)
    - [获取Git仓库](#获取git仓库)
        - [在现有目录中初始化仓库](#在现有目录中初始化仓库)
        - [克隆现有的仓库](#克隆现有的仓库)
    - [记录每次更新到仓库](#记录每次更新到仓库)
        - [检查当前文件状态](#检查当前文件状态)
        - [跟踪新文件和暂存已修改文件](#跟踪新文件和暂存已修改文件)
        - [状态简览](#状态简览)
        - [忽略文件](#忽略文件)
        - [查看已暂存和未暂存的修改](#查看已暂存和未暂存的修改)
        - [提交更新](#提交更新)
        - [跳过使用暂存区域](#跳过使用暂存区域)
        - [移除文件](#移除文件)
    - [查看提交历史](#查看提交历史)

<!-- /TOC -->
# Pro_Git

# Git 初次配置

安装后的第一件事情：配置用户信息

``` git
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

> 如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

如果要检查你的配置信息：
``` git 
$ git config --list
$ git config user.name
```

# Git基础

## 获取Git仓库
有两种取得 Git 项目仓库的方法。 第一种是在现有项目或目录下导入所有文件到 Git 中； 第二种是从一个服务器克隆一个现有的 Git 仓库。

### 在现有目录中初始化仓库

如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入：
``` git
$ git init

//提交作为此项目的第一个版本
$ git add -A
$ git commit -m 'initial project version'
```

### 克隆现有的仓库
克隆仓库的命令格式是 git clone [url] 。比如，要克隆 Git 的可链接库 libgit2，可以用下面的命令：
``` git
$ git clone https://github.com/libgit2/libgit2
```
如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以使用如下命令：
``` git
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

## 记录每次更新到仓库
作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复

### 检查当前文件状态

``` git 
$ git status

//输出 说明工作目录未做出修改
On branch master
nothing to commit, working directory clean
```

### 跟踪新文件和暂存已修改文件

``` git
//跟踪 text文件
$ git add text
```

> 执行 `git commit` 命令的时候提交的是 `git add` 命令时的版本，而不是 `git commit` 命令的版本。

### 状态简览

`git status` 命令的输出十分详细，但其用语有些繁琐。 如果你使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种更为紧凑的格式输出。 运行 `git status -s` ，状态报告输出如下：

``` git
$ git status -s

//输出如下
 M README   //M 出现在右边
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb //M 出现在左边
?? LICENSE.txt
```
- M 右边:表示该文件被修改了但是还没放入暂存区
- M 左边:该文件被修改了并放入了暂存区
- A 新加进缓存区的文件
- MM 表示该文件被加进缓存区后又被修改了
- ?? 新添加的未跟踪文件

### 忽略文件

创建一个名为 .gitignore 的文件，列出要忽略的文件模式。 
文件 .gitignore 的格式规范如下：

- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

这里推荐一个[仓库](https://github.com/github/gitignore)，列出了各种语言项目要忽略的文件的`.gitignore`文件

### 查看已暂存和未暂存的修改

``` git
$ git diff

//上面这个命令是进入了vim，按`q`退出
```

### 提交更新

在提交之前确保你的文件都在git的暂缓存区了；用`git status`查看，提交：
``` git
$ git commit
//输入这个命令会进入vim，按esc后，输入：`:q` 回车退出。
```
通常的提交我们应该填写修改了些什么内容这里就是让我们修改，会vim就可以直接在vim填写修改说明。

不会vim也没关系，我们用组合命令：
``` git
$ git commit -m "第一次修改"
//这里我们加入 `-m` 来填写提交修改说明。
```

### 跳过使用暂存区域

我们提交一个文件每次要操作两步，`git add` 添加到暂缓存区，`git commit` 提交；git提供一个命令把两个步骤合并起来：
``` git
$ git commit -a -m "提交说明"
//提交之前不再需要 git add ，就可以提交最新的修改了
```
### 移除文件
执行完 `git commit` 后发现多了个不想提交的文件，这里使用：

``` git
$ git rm '文件名'

//输出
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    test.txt

```
再次提交的时候就提示：
``` git 
$ git commit -a -m "删除文件"
[master b71ea57] 删除文件
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```
删除成功。

> `git rm` 删除会连同本地文件一起删除。

如果添加进缓存区，还没有提交，`git rm` 是无法删除的，要使用：
``` git 
$ git rm -f "文件名"
//强制删除
```

## 查看提交历史