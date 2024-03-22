# git for windown
## 概念
**git有三种状态**
- 已修改(modified)，表示修改了文件，但还没保存到数据库中
- 已暂存(staged)，表示对一个已修改的文件做了标记，使之包含在下次提交的快照[^1]中
- 已提交(committed)，表示数据已经安全保存在本地数据库中
  
[^1]:快照（待补充）

**git基本工作流程**
![](img/2024-03-21-10-19-03.png '工作目录、暂存区域、git仓库')
1. 在工作区修改文件(modified)
2. 将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区(staged)
3. 提交更新，找到暂存区的文件，将快照永久性存储到Git目录(committed)

## 基础流程
### 1. 配置并初始化一个仓库(repository)
- 在已存在目录中初始化仓库(空，无跟踪文件)
   git init
- 克隆现有的仓库
   git clone

### 2. 开始或停止跟踪(track)文件、暂存(stage)或提交(commit)更改
工作目录下每个文件根据是否纳入版本控制(git已经知道)分为**已跟踪**or**未跟踪**
![](img/2024-03-21-16-13-57.png '文件状态变化周期')
- 查看当前文件状态
  git status

- 跟踪文件暂存区(将内容添加到下一次提交中)
  git add

- 查看修改
  - 比较工作目录中文件(已修改/未修改)和暂存区文件的差异
    git diff
  - 比较暂存区文件和git仓库文件的差异
    git diff --staged/--cached

- 提交更新
  git commit

- 移除文件(停止追踪)
  - 有保护机制，加-f包括源文件删除
    git rm -f
  - 加--ached，仅从git仓库删除
    git rm --ached

- 移动/重命名文件
  git mv

### 3. 配置git来忽略指定的文件
- 忽略文件(使用失败，待厘清)
  - 创建一个.gitignore文件
    ```
    # 忽略所有的 .a 文件
    *.a
    # 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
    !lib.a
    # 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
    /TODO
    # 忽略任何目录下名为 build 的文件夹
    build/
    # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
    doc/*.txt
    # 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
    doc/**/*.pdf
    ```
  - 文件.gitignore的格式规范
    - 所有空行或者以`#`开头的行都会被git忽略
    - 可以使用标准的glob[^2]模式匹配，它会递归地应用在整个工作区中
    - 匹配模式可以`/`开头防止递归
    - 匹配模式可以`/`结尾指定目录
    - 要忽略指定模式以外的文件或目录，可以在模式前加上`!`取反

    [^2]:指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（\*\*）表示匹配任意中间目录，比如 a/\*\*/z 可以匹配 a/z 、 a/b/z 或 a/b/c/z 等。


配置git来忽略指定的文件和文件模式、迅速而简单地撤销误操作、如何浏览你的项目的历史版本以及不同提交(commits)之间的差异、如何向你的远程仓库推送(push)、如何从你的远程仓库拉取(pull)文件。

[快速跳转](#快速跳转)
# 快速返回

# git命令
## 版本更新
2.17.1之前的，使用
`git update`
2.17.1之后的，使用
`git update-git-for-windows`
其它，[官网下载](https://git-scm.com/download/win)

## 帮助
`git help`
`git help <verb>` or `git <verb> -h`
- `verb` e.g. config, clone, add...
## git config
### 查看所有配置
`git config -l`
及其所在的文件:
`git config --list --show-origin`

### 查看指定配置
`git config <key>`
- `key` e.g. user.name...

### 设置用户信息
使用了`--global`选项一次，该命令对全局生效，否则仅对工作目录生效
```
git config --global user.name 'hongjiong'
git config --global user.email hongjiong_zhu@163.com
```

## git init
在当前目录下创建一个.git

## git add
添加需追踪的文件
```
$ git add *.c
$ git add LICENSE
```

## git commit
提交追踪的文件到仓库中，进行版本控制
```
$ git commit -m 'initial project version'
```

## git clone
克隆远程仓库每一个文件，每一个版本
`git clone <url> <myLibName>` 
- `<url>`指远程仓库路径
- `<myLibName>`指克隆库本地命名，不写默认远程仓库名
```
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

## git status
查看目录下文件状态
```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        111.md
        DevSidecar-1.7.3.exe
        Git-2.43.0-64-bit.exe
        img/
        progit.pdf

nothing added to commit but untracked files present (use "git add" to track)

```
信息繁琐用`git status -s`
```
$ git status -s
AM 111.md
?? DevSidecar-1.7.3.exe
?? Git-2.43.0-64-bit.exe
?? img/
?? progit.pdf
```
输出信息有两栏，左栏为暂存区状态，右栏为工作区状态
- `??`为未跟踪文件
- `A`为新添加到暂存区文件
- `M`为修改过文件
- `111.md`为暂存后又做了修改文件

## git add
跟踪文件到暂存区(将内容添加到下一次提交中)
```
$ git add 111.md
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   111.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        DevSidecar-1.7.3.exe
        Git-2.43.0-64-bit.exe
        img/
        progit.pdf

```

## git diff
请注意，git diff本身只显示未暂存的改动
```
$ git diff
diff --git a/111.md b/111.md
index 56ed569..05d52b3 100644
--- a/111.md
+++ b/111.md
@@ -25,16 +25,40 @@
 ![](img/2024-03-21-16-13-57.png '文件状态变化周期')
 - 查看当前文件状态
   git status
+
 - 跟踪文件暂存区(将内容添加到下一次提交中)
   git add
-- 忽略文件(使用失败，代理情)
-  创建一个.gitignore文件
-  ```
-  $ cat .gitignore
-  *.[oa]
-  *~
-  ```

+- 忽略文件(使用失败，待厘清)
+  - 创建一个.gitignore文件
+    ```
+    # 忽略所有的 .a 文件
```
用 git diff --staged/--cached 查看已经暂存起来的变化
```
$ git diff --staged
diff --git a/111.md b/111.md
index 2c8bfc0..56ed569 100644
--- a/111.md
+++ b/111.md
@@ -27,6 +27,13 @@
   git status
 - 跟踪文件暂存区(将内容添加到下一次提交中)
   git add
+- 忽略文件(使用失败，代理情)
+  创建一个.gitignore文件
+  ```
+  $ cat .gitignore
+  *.[oa]
+  *~
+  ```


 配置git来忽略指定的文件和文件模式、迅速而简单地撤销误操作、如何浏览你的项目的历史版本以及不同提交(commits)之间的差异、如何向你的远程仓库推送(push)、如何从你的
远程仓库拉取(pull)文件。
@@ -149,3 +156,4 @@ Untracked files:

 # 待处理
```
- 配置difftool
  git difftool --tool-help

## git commit
将暂存区文件提交到git仓库
`git commit -m "this commit log"`将文件与log同时提交
`git commit -a`将跟踪过的文件提交，跳过暂存动作
`git commit -v`将此次提交文件diff呈现在编辑器中
```
$ git commit -m "commit test 3rd by John"
[master 9d285b2] commit test 3rd by John
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 progit.pdf

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   111.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .111.md.swp
        DevSidecar-1.7.3.exe
        Git-2.43.0-64-bit.exe

no changes added to commit (use "git add" and/or "git commit -a")

$ git add 111.md

$ git commit -m "commit test 4th by John"
[master 58830be] commit test 4th by John
 1 file changed, 95 insertions(+), 7 deletions(-)

$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .111.md.swp
        DevSidecar-1.7.3.exe
        Git-2.43.0-64-bit.exe

nothing added to commit but untracked files present (use "git add" to track)

```

## git rm
要从Git中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除，然后提交
- `git rm`会连带删除源文件
- 移除暂存区的文件时，会有保护机制，需要`git rm -f`，也会连带删除源文件
    ```
    $ git rm 111.md
    error: the following file has changes staged in the index:
        111.md
    (use --cached to keep the file, or -f to force removal)

    $ git rm -f 111.md
    rm '111.md'

    $ git status
    On branch master
    Untracked files:
    (use "git add <file>..." to include in what will be committed)
            .111.md.swp
            DevSidecar-1.7.3.exe
            Git-2.43.0-64-bit.exe

    nothing added to commit but untracked files present (use "git add" to track)

    ```
- `git rm -cached`仅移除暂存区
  ```
  $ git status
    On branch master
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            new file:   111.md

    Untracked files:
    (use "git add <file>..." to include in what will be committed)
            .111.md.swp
            DevSidecar-1.7.3.exe
            Git-2.43.0-64-bit.exe

    $ git rm --cached 111.md
    rm '111.md'

    $ git status
    On branch master
    Untracked files:
    (use "git add <file>..." to include in what will be committed)
            .111.md.swp
            111.md
            DevSidecar-1.7.3.exe
            Git-2.43.0-64-bit.exe

    nothing added to commit but untracked files present (use "git add" to track)

  ```
  - 多文件移除`git rm <glob>`[^2]
  ```
  # 移除log文件夹里所有.log文件
  $ git rm log/\*.log
  # 移除所有~结尾文件
  $ git rm git rm \*~
  ```

## git mv
重命名or移动
```
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
(use "git reset HEAD <file>..." to unstage)
renamed: README.md -> README
```
相当于
```
$ mv README.md README
$ git rm README.md
$ git add README
```

# 快速跳转
[快速返回](#快速返回)

# 待处理
- [ ] .git里文件信息参考git内部原理
- [ ] .gitignore忽略文件使用失败
- [ ] 配置difftool