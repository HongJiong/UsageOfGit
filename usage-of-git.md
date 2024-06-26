# git for windown
记录一些会用到的操作/功能
# 概念
- git有三种状态
  - 已修改(modified)，表示修改了文件，但还没保存到数据库中
  - 已暂存(staged)，表示对一个已修改的文件做了标记，使之包含在下次提交的快照[^1]中
  - 已提交(committed)，表示数据已经安全保存在本地数据库中
  
[^1]:快照（待补充）

- Git作为一个系统来管理并操纵三棵树(文件的集合):\
  |树   |用途   |
  |---|---|
  |HEAD   |上一次提交的快照,下一次提交的父节点   |
  |Index   |预期的下一次提交的快照(暂存区)   |
  |WorkingDirectory   |沙盒/工作目录   |

**git基本工作流程**\
![](img/2024-03-21-10-19-03.png '工作目录、暂存区域、git仓库')
1. 在工作区修改文件(modified)
2. 将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区(staged)
3. 提交更新，找到暂存区的文件，将快照永久性存储到Git目录(committed)

# 基础流程
## 1.配置并初始化一个仓库(repository)
- 在已存在目录中初始化仓库(空，无跟踪文件)\
   `git init`
- 克隆现有的仓库\
   `git clone`

## 2.开始或停止跟踪(track)文件、暂存(stage)或提交(commit)更改
工作目录下每个文件根据是否纳入版本控制(git已经知道)分为**已跟踪**or**未跟踪**
![](img/2024-03-21-16-13-57.png '文件状态变化周期')
- 查看当前文件状态\
  `git status`

- 跟踪文件暂存区(将内容添加到下一次提交中)\
  `git add`

- 查看修改
  - 比较工作目录中文件(已修改/未修改)和暂存区文件的差异\
    `git diff`
  - 比较暂存区文件和git仓库文件的差异\
    `git diff --staged/--cached`

- 提交更新\
  `git commit`

- 移除文件(停止追踪)
  - 有保护机制，加-f包括源文件删除
    `git rm -f`
  - 加--ached，仅从git仓库删除
    `git rm --ached`

- 移动/重命名文件\
  `git mv`

- 撤销
  - 撤销提交，新的提交替换旧的提交\
    `git commit --amend`\
    - 加`--no-edit`新提交使用旧提交的日志
  - 撤销添加暂存\
    `git reset HEAD <file>`
  - 撤销修改\
    `git checkout - <file>`

## 3.配置git来忽略指定的文件
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

## 4.历史
- 查看提交信息\
  `git show <hashHistory>`
  - 查看当前版本的父提交\
    `git show HEAD^^`\
    或 `git show 'HEAD^'`\
    或 `git show ~HEAD`
  - 查看当前版本的第n代提交\
    `git show HEAD~<n>`\
    或 `git show HEAD<n * '~'>`

- 查看提交历史，不传入参数按时间先后顺序列出所有的提交，列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。\
  `git log`
  ```
  commit 18dac74e6875ed1da275f53c1762cd8aca675fdc (HEAD -> master, origin/master, origin/HEAD)
  Author: HongJiong <135309490+HongJiong@users.noreply.github.com>
  Date:   Fri Mar 22 18:37:48 2024 +0800

    Add files via upload

  commit 96c7015d90f07720a0de5a51e5b7438ccec62e5f
  Author: HongJiong <hongjiong_zhu@163.com>
  Date:   Fri Mar 22 18:19:14 2024 +0800

    add img for usage-of-github
  ```

- 查看历史HEAD和分支引用所指向的历史
  `git reflog`
  ```
  $ git reflog
  734713b HEAD@{0}: commit: fixed refs handling, added gc auto, updated
  d921970 HEAD@{1}: merge phedders/rdocs: Merge made by the 'recursive'
  strategy.
  1c002dd HEAD@{2}: commit: added some blame and merge stuff
  1c36188 HEAD@{3}: rebase -i (squash): updating HEAD
  95df984 HEAD@{4}: commit: # This is a combination of two commits.
  1c36188 HEAD@{5}: rebase -i (squash): updating HEAD
  7e05da5 HEAD@{6}: rebase -i (pick): updating HEAD
  ```
  - 使用`@{n}`来引用reflog中提交记录\
    `git show HEAD@{5}`查看五次前所指向的提交

- 标签
  - 列出已有\
    `git tag`\
    匹配"v1.8.5"开头的标签\
    `git tag -l "v1.8.5*"`

  - 创建
    - 轻量标签\
      `git tag <tagName>`
    - 附注标签，是存储在 Git 数据库中的一个完整对象， 可以被校验，包含打标签者的名字、电子邮件地址、日期时间，还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证\
      `git tag -a <tagName> -m "tagLog"`
    - 后期标签，给已提交的版本打标\
      `git tag -a <tagName> <hash>`\
      `git tag <tagName> <hash>`

  - 输出信息\
    `git show <tagName>`

  - 远程共享
    - `git push`默认不会传标签到远程仓库，需要：\
      `git push <libName> <tagName>`
    - 推送所有标签\
      `git push <libName> --tags`

  - 删除\
    `git tag -d <tagName>`
    - 推送删除的标签\
      `git push <libName> :refs/tags/<tagName>`\
      `git push <libName> --delete <tagName>`

## 5.远程仓库
- 查看\
  `git remote`\
  `git remote show <libName>`

- 添加\
  `git remote add <libName> <url>`

- 拉取\
  `git fetch <libName>`\
  `git pull`

- 推送\
  `git push <libName> <branchName>`

- 重命名\
  `git remote rename oldLibName newLibName`

- 移除\
  `git remote remove libName`\
  `git remote rm libName`

## 6.别名
- 创建git cmd别名\
  `git config [level] alias.<newName> <cmdName>`

- 查看定义的别名\
  `git config --global -l | grep alias`

- 取消别名\
  `git config --global --unset alias.<newName>`

# 分支
Git的分支，其实本质上仅仅是**指向提交对象的可变指针**，通常称`HEAD`。Git的默认分支名字是 master，与其它分支没有区别。

## 查看
- 获取所有分支列表，`*`代`表HEAD->`\
  `git branch`
  ```
  $ git branch
    iss53
  * master
    testing
  ```
- 获取每个分支最后一次提交\
  `git branch -v`
- 过滤已/未合并分支\
  `git branch --merged/--no-merged`
- 获取每个分支详细信息\
  `git branch -vv`
  ```
  $ git branch -vv
    iss53 7e424c3 [origin/iss53: ahead 2] forgot the brackets
    master 1ae2a45 [origin/master] deploying index fix
  * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
    testing 5ea463a trying something new
  ```
  - `ahead 2`代表本地还有两个提交还没有推送到服务器上
  - `ahead 3, behind 1`代表本地还有三个提交还没有推送到服务器上，服务器有一次提交没有合入到本地
  - 服务器信息基于本地，所以要先`git fetch --all`

- 查看branch1中有哪些不在branch0的提交中\
  `git log <branch0> <branch1>`

## 新分支
- 创建，从`HEAD`看出没有切换到新分支\
  `git branch <branchName>`
  ![](img/2024-03-25-17-46-25.png)
- 切换，从`HEAD`看出切换到新分支\
  `git checkout <branchName>`
- 创建并切换\
  `git checkout -b <branchName>`
  ![](img/2024-03-25-17-48-32.png)

## 删除
- 只能删除未合并分支\
  `git branch -d <branchName>`

## 合并
把两个分支的最新快照（C3 和 C4）以及二者最近的共同祖先（C2）进行三方合并，合并的结果是生成一个新的快照（并提交）。\
![](img/2024-03-26-14-35-31.png)

- 将所在分支与目标分支合并，并自动提交\
  `git merge <targetBranchName>`
  - fast-forward:合并(master与hotfix)没有要解决的冲突
    ![](img/2024-03-25-18-24-54.png)
  - 不同父继承合并
    |![](img/2024-03-25-18-28-30.png)||
    |:---:|---|
    |↓||
    |![](img/2024-03-25-18-29-10.png)||

- 冲突合并：需合并的两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，不会自动提交
  1. `git merge`
    ```
    $ git merge iss53
    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.
    ```
  2. 用git status查看冲突(unmerged)信息
    ```
    $ git status
    On branch master
    You have unmerged paths.
      (fix conflicts and run "git commit")

    Unmerged paths:
      (use "git add <file>..." to mark resolution)

        both modified:      index.html
    no changes added to commit (use "git add" and/or "git commit -a")
    ```
  3.  打开冲突文件，其中包含以而写特殊区段
    > `=======`的上半部分为当前分支(HEAD -> master)的index.html文件，下半部分为目标分支(iss53)的index.html文件
    ```
    <<<<<<< HEAD:index.html
    <div id="footer">contact : email.support@github.com</div>
    =======
    <div id="footer">
    please contact us at support@github.com
    </div>
    >>>>>>> iss53:index.html
    ```
  4.  手动更改冲突文件，并对其`git add`暂存，就能解决冲突，再`git commit`
  
- 放弃冲突: 有合并冲突时, `git merge --abort`可以恢复到合并前的状态 
- 忽略空白冲突: `git merge --Xignore-all-space/--Xignore-space-change`

## 远程
- 推送\
  `git push <libName> <branchName>`

- 抓取\
  `git fetch -all`

  - 抓取新的远程跟踪分支`newBranch`时，本地不会有一个新的分支`newBranch`，只有一个不可以修改的`origin/newBranch`指针，需要：
    - 将newBranch合并到当前分支，在当前分支上工作：\
      `git merge origin/newBranch`
    - 在上游(远程)分支上检出跟踪(本地)分支，起点位于`origin/newBranch`：\
      `git checkout -b newBranch origin/newBranch`\
      或 `git checkout --track origin/newBranch`\
      或 如果上游分支在本地不存在，可以直接`git checkout newBranch`
    
  - 如果在跟踪分支上`git pull`，能正确抓取上游分支

  - 通过`@upstream`或`@u`代替当前分支跟踪的上游分支\
    `git merge @u`相当与`git merge origin/master`

- 删除\
  `git push origin --delete <branchName>`

## 变基
- 将单个提交中的修改作为一个新的提交引入到当前的分支上/
  `git cherry-pick <commitedID>`/
  ![](img/2024-03-27-18-09-04.png)

  
- 将提交到某一分支上的所有修改都移至另一分支上，操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交，**会改变提交历史**
  - 提取在 C4 中引入的补丁和修改，然后在 C3 的基础上应用一次
    1. 检出experiment分支，然后变基到master分支上
      ~~~
      $ git checkout experiment
      $ git rebase master
      First, rewinding head to replay your work on top of it...
      Applying: added staged command
      ~~~
      ![](img/2024-03-26-14-47-53.png)
    2. 回到master进行一次合并
      ~~~
      $ git checkout master
      $ git merge experiment
      ~~~
      ![](img/2024-03-26-14-52-21.png)

- 将`client`中的修改合并到`master`，但暂时并不想合并`server`中的修改，即 C8 和 C9
  ~~~
  $ git checkout client
  $ git rebase --onto master server client
  ~~~
  ![](img/2024-03-26-15-59-12.png)

# Git Tool
## 交互式暂存
打开交互式终端, 输出与`git status`基本相同的信息, 左侧为暂存区, 右侧为未暂存的修改文件, 在`What now`输入相应cmd进行git文件操作\
- `git add -i` 或 `git add --interactive`
  ```
  $ git add -i
           staged   unstaged  path
    1:  unchanged      +0/-1  TODO
    2:  unchanged      +1/-1  index.html
    3:  unchanged      +5/-1  lib/simplegit.rb
  *** Commands ***
    1: [s]tatus   2: [u]pdate   3: [r]evert   4: [a]dd untracked
    5: [p]atch    6: [d]iff     7: [q]uit     8: [h]elp
  What now>
  ```

- 暂存与取消暂存文件
  - `What now`键入`u`或`2`, 选择要暂存哪些文件(`TODO` & `index.html`)
  - `Update>>`直接回车退出, 并暂存`*`代表的已选择文件
    ```
    What now> u
            staged unstaged path
      1: unchanged    +0/-1 TODO
      2: unchanged    +1/-1 index.html
      3: unchanged    +5/-1 lib/simplegit.rb
    Update>> 1,2
              staged unstaged path
      * 1: unchanged    +0/-1 TODO
      * 2: unchanged    +1/-1 index.html
        3: unchanged    +5/-1 lib/simplegit.rb
    Update>> \n
    updated 2 paths
    ```
  - 键入`r`或`3`, 选择取消暂存哪些文件, 操作如上
  - 键入`d`或`6`, 显示暂存区别, 类似于`git diff --cached` 
  - 键入`p`或`5`, 选择部分暂存哪些文件，对已选择文件的每一个部分会一一显示文件区别并询问是否想要暂存它们(暂存补丁)
    - 键入`?`可以显示所有可以使用的命令列表

## 贮藏
- **跟踪文件**的修改与暂存的改动, 将其保存到一个栈上, 然后可以在不同的分支上重新应用这些改动\
  `git stash` 或 `git stash push`
  ```
  $ git stash
  Saved working directory and index state \
  "WIP on master: 049d078 added the index file"
  HEAD is now at 049d078 added the index file
  (To restore them type "git stash apply")
  ```
  - 加`--include-untracked` 或 `-u`贮藏未跟踪文件, 不包括忽略文件
  - 加`--all` 或 `-a`贮藏所有文件

- 查看\
  `git stash list`
  ```
  $ git stash list
  stash@{0}: WIP on master: 049d078 added the index file
  stash@{1}: WIP on master: c264051 Revert "added file_size"
  stash@{2}: WIP on master: 21d80a5 added number to log
  ```

- 应用\
  `git stash apply`
  ```
  $ git stash apply
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
      modified:   index.html
      modified:   lib/simplegit.rb
  no changes added to commit (use "git add" and/or "git commit -a")
  ```
  - 默认应用最近一个, 其它可以加引用, 如`git stash apply stash@{2}`
  - 由于贮藏只有`有修改与未提交的文件`, 在不同分支应用时之前暂存的文件却没有重新暂存, 必须`git stash apply --index`回到原来的位置来尝试重新应用暂存的修改

- 移除\
  `git stash drop stash@{n}`

- 应用并移除\
  `git stash pop`

- 用贮藏创建分支\
  `git stash branch <branchName>`
  - 创建一个新分支并检出贮藏工作时所在的提交, 重新在那应用工作, 应用成功后丢弃贮藏
    ```
    git stash branch testchanges
    M   index.html
    M   lib/simplegit.rb
    Switched to a new branch 'testchanges'
    On branch testchanges
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
        modified:   index.html
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   lib/simplegit.rb
    Dropped refs/stash@{0} (29d385a81d163dfd45a452a2ce816487a6b8b014)
    ```

## 清理工作目录
从工作目录移除未被追踪的文件
`git clean -d`
- 加`-n` 或 `--dry-run`, 不会移除, 输出要删除什么
- 加`-f`强制移除
- 加`-x`移除忽略文件
- 加`-i` 或 `--interactive`, 交互模式

## 文件标注
查看文件来自哪个提交和提交者\
`git blame <file>`
- 限制文件69到82行\
  `git blame -L 68,82 MakeFile`



# SSH
**[绑定主机，在设置的权限下免密操作github](./usage-shh-token.md#ssh)**

[网上说明](https://blog.csdn.net/weixin_42310154/article/details/118340458)

# 快速返回
[快速跳转](#快速跳转)


# git命令
## 版本更新
2.17.1之前的，使用\
`git update`\
2.17.1之后的，使用\
`git update-git-for-windows`\
其它，[官网下载](https://git-scm.com/download/win)

## 帮助
`git help`\
`git help <verb>` or `git <verb> -h`\
- `verb` e.g. config, clone, add...

## 配置
- 查看所有配置\
  `git config -l`\
  及其所在的文件:\
  `git config --list --show-origin`

- 查看指定配置\
  `git config <key>`
  - `key` e.g. user.name...

- 设置用户信息
  使用了`--global`选项一次，该命令对全局生效，否则仅对工作目录生效
  ```
  git config --global user.name 'hongjiong'
  git config --global user.email hongjiong_zhu@163.com
  ```

- 设置cmd别名
  - 加`[level] alias.<newName> <cmdName>`创建cmd别名
  - 加`[level] -l | grep alias`查看定义别名
  - 加`[level] --unset alias.<newName>`取消别名
  - e.g.`git config --global alias.s 'status -s'`

- 设置凭证助手，无需每次远程操作输入账号密码\
  `git config --global credential.helper cache`
  - 加`cache`, 账号密码放在内存中, 时效15min
  - 加`store`, 账号密码放在磁盘中, 时效永久

## 初始化
- 在当前目录下创建一个.git\
  `git init`

## 克隆
- 克隆远程仓库每一个文件，每一个版本\
  `git clone <url> <myLibName>` 
  - `<url>`指远程仓库路径
  - `<myLibName>`指克隆库本地命名，不写默认远程仓库名
  ```
  $ git clone git@github.com:HongJiong/UsageOfGit.git origin
  ```

## 文件状态
- 查看目录下文件状态\
  `git status`
- 信息繁琐用加`-s`
  ```
  $ git status -s
  AM usage-of-git.md
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

## 添加
- 添加文件到暂存区(添加到下一次提交中)
  ```
  $ git add *.c
  $ git add LICENSE
  ```
- 打开交互式终端, 输出与`git status`基本相同的信息, 左侧为暂存区, 右侧为未暂存的修改文件, 在`What now`输入相应cmd进行git文件操作
  ```
  $ git add -i
           staged   unstaged  path
    1:  unchanged      +0/-1  TODO
    2:  unchanged      +1/-1  index.html
    3:  unchanged      +5/-1  lib/simplegit.rb
  *** Commands ***
    1: [s]tatus   2: [u]pdate   3: [r]evert   4: [a]dd untracked
    5: [p]atch    6: [d]iff     7: [q]uit     8: [h]elp
  What now>
  ```

## 提交
- 提交暂存区文件到.git仓库\
  `git commit`
  - 加`-m "this is commit log"`将文件与Log同时提交
  - 加`-a`将在暂存区文件清单的文件提交，跳过添加动作
  - 加`-v`将此次提交文件diff呈现在编辑器中
  - 加`--amend`将此次提交代替上一次提交，撤销提交

## 差异
- 请注意，`git diff`本身只显示未暂存的改动
- 加`--staged`或`--cached`查看已经暂存起来的变化
- 配置difftool
  `git difftool --tool-help`

## 移除
要从git中移除某个文件，就必须要从暂存区文件清单中移除，然后提交
- `git rm`会连带移除源文件
- 移除暂存区的文件时，会有保护机制，需要`git rm -f`，也会连带移除源文件
- `git rm -cached`仅移除暂存区
- 多文件移除`git rm <glob>`[^2]

## 移动/重命名
- `git mv <pathSrc> <pathDes>`
  相当于
  ```
  $ mv <pathSrc> <pathDes>
  $ git rm <pathSrc>
  $ git add <pathDes>
  ```

## 重置
`git reset`命令按以下顺序重写三棵树, 加选项停止
![](img/2024-03-27-16-04-19.png)
1. 移动`HEAD`指向分支的指向(撤销上一次提交), 加`--soft`到此停止\
   `git reset --soft HEAD~`\
   ![](img/2024-03-27-16-05-40.png)
2. 使Index(暂存区)看起来像HEAD(撤销上一次提交与暂存), 默认或者加`--mixed`到此停止\
   `git reset --mixed HEAD~`\
   ![](img/2024-03-27-16-08-33.png)
3. 使工作目录看起来像Index(撤销上一次所有操作, **无法恢复**), 加`--hard`才会到此\
   `git reset --hard HEAD~`\
   ![](img/2024-03-27-16-11-27.png)

- 加文件路径不会动`HEAD`(局部回滚), 重置/撤销暂存文件\
  `git reset (--hard/--mixed) HEAD <file>`

## 检出
- 不带文件路径时, 移动`HEAD`本身的指向，并将工作目录/文件恢复到`HEAD`指向的分支版本(相应分支最后一次提交)，原内容未提交不会保存\
  `git checkout`\
  ![](img/2024-03-27-16-34-17.png)
  - 加`<branchName>`切换分支, 相当于`git reset --hard <branchName>`
  - 加`-b <branchName>`创建并切换分支

- 带文件路径时, 不会移动`HEAD`, 重置文件\
  `git checkout HEAD - <file>`相当于`git reset --hard HEAD <file>`

## 还原提交
反向cherry-pick操作, 将父提交当成新提交合入主线\
`git revert -m 1`
- `-m <n>`指`mainline`要保留下来的分支, 撤销合并会使用到
![](img/2024-03-27-18-35-30.png)
- `-m 1`指`C6`所在分支, revert出的`^M`与`C6`完全一样

## 查看提交历史
- `git log`
- 常用选项|说明
  ---|---
  -g | 带上reflog
  -p | 按补丁格式显示每个提交引入的差异
  --stat | 显示每次提交的文件修改统计信息
  --shortstat | 只显示 --stat 中最后的行数修改添加移除统计
  --name-only | 仅在提交信息后显示已修改的文件清单
  --name-status | 显示新增、修改、删除的文件清单
  --abbrev-commit | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符
  --relative-date | 使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）
  --graph 在日志旁以 | ASCII 图形显示分支与合并历史
  --pretty | 使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和 format（用来定义自己的格式）
  --abbrev-commit | 显示简短且唯一的值
  --oneline | --pretty=oneline --abbrev-commit 合用的简写
  限制输出选项 |
  -<n> | 仅显示最近的 n 条提交。
  --since, --after | 仅显示指定时间之后的提交
  --until, --before | 仅显示指定时间之前的提交
  --author | 仅显示作者匹配指定字符串的提交
  --committer | 仅显示提交者匹配指定字符串的提交
  --grep | 仅显示提交说明中包含指定字符串的提交
  -S | 仅显示添加或删除内容匹配指定字符串的提交

  e.g.
  ```
  $ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" --before="2008-11-01" --no-merges -- t/
  5610e3b - Fix testcase failure when extended attributes are in use
  acd3b9e - Enhance hold_lock_file_for_{update,append}() API
  f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
  d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
  51a94af - Fix "checkout --track -b newbranch" on detached HEAD
  b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
  ```

  - 查看分支历史
    ```
    $ git log --oneline --decorate --graph --all
    * c2b9e (HEAD, master) made other changes
    | * 87ab2 (testing) made a change
    |/
    * f30ab add feature #32 - ability to add new formats to the
    * 34ac2 fixed bug #1328 - stack overflow under certain conditions
    * 98ca9 initial commit of my project
    ```
- 查看`HEAD`和分支引用所指向的历史\
  `git reflog`

## 远程操作
`git remote`查看LibName
- 加`show <libName>`或`-v`查看详细信息
- 加`add <libName> <url>`添加仓库
- 加`rename oldLibName newLibName`仓库重命名
- 加`remove libName`或`rm`移除仓库看

## 远程拉取
- 从服务器上抓取本地没有的数据，可以随时合并或查看\
  `git fetch <libName>`
  - 拉取服务所有信息\
    `git fetch --all`
- 从服务器上抓取本地没有的数据，并合并\
  `git pull`，相当于`git fetch` + `git merge`

## 远程推送
- 将本地仓库信息推送到远程仓库\
  `git push <libName>`
  - 加`<branchName>`推送分支

  - 加`<tagName>`将远程共享标签
  - 加`--tags`将远程共享所有标签
  - 加`:refs/tags/<tagName>`或`--delete <tagName>`远程删除标签

  - 加`--delete <branchName>`删除对应分支
  
## 标签
`git tag`列出已有
- 加`-l "v*"`列出匹配"v"开头的标签
- 加`<tagName>`创建轻量标签
- 加`-a <tagName> -m "tagLog"`创建附注标签
  - 创建标签再加`<hashHistory>`给历史提交打标
- 加`-d <tagName>`删除标签

## (git show)
`git show`
- 加`<tagName>`打印标签信息
- 加`HEAD@{5}`查看HEAD在5次前指向的提交
- 加

## 分支
获取所有分支列表\
`git branch`
- 加`--`merged/--no-merged过滤已/未合并分支
- 加`-v`获取每个分支最后一次提交
- 加`-vv`获取每个分支详细信息
- 加`-d <branchName>`删除未合并分支

## 合并
将所在分支与目标分支合并，并自动提交
`git merge <targetBaranchName>`

## 变基
提取当前分支的修改合入目标分支\
`git rebase <targetBranch>`
- 提取当前分支中不包含上游分支的修改合入目标分支\
  `git rebase --onto <targetBranch> <upstreamBranch> <currentBranch>`

# 快速跳转
[快速返回](#快速返回)

# 待处理
- [ ] .git里文件信息参考git内部原理
- [ ] .gitignore忽略文件使用失败
- [ ] 配置difftool
