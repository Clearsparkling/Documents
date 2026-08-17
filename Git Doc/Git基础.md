# Git

## Git基础命令

### 初始化git仓库

``` shell
cd "/url"
git init
```

### 克隆现有仓库

``` shell
git clone <url>
```

### 添加远程仓库

``` shell
git remote add <shortname> <url>	

// 首次向远程仓库推送代码
// 将本地分支与远程分支相关联，以后只需要git push即可
git push -u origin master
```

### 拉取代码

#### 默认拉取当前分支

``` shell
git pull

// 在没有设置upstream的时候 主动选择从哪里拉去到本地的指定分支
git pull origin master
```

### 暂存

#### 暂存文件

``` shell
// 暂存指定文件
git add <file>

// 暂存所有文件
git add .
```

#### 版本回退

撤销这一次的`git add`,但文件仍被git跟踪，修改还保留在工作区，随时可以`git add`

``` shell
# git reset --mixed 版本号 -- 文件名
git reset HEAD<file>
```

从版本库中将指定版本恢复到暂存区和工作区

```shell
# git reset --hard 版本号 -- 文件名
git reset --hard<file>
```

从版本库中撤销提交，但修改的内容仍保存在工作区和暂存区中，随时可以重新`commit`

>[!WARNING]
>
>此操作只建议在未push到远程分支的commit使用，这会撤销这之中版本的所有提交，如果对远程分支如此使用，会缺少中途的提交记录，此时提交会被拒绝，如需强行提交使用`git push --force-with-lease`，但如果此时已经有人拉取此代码在进行协同开发会问题
>
>如需要撤销已经push的commit使用
>
>`git revert <version>`
>
>会新增一次反向提交拥有完整的提交记录

```shell
# git reset --soft 版本号 -- 文件名
git reset --soft <versionhash> -- <file>
```

在本地reset之后，未来的分支记录依然会保存在本地，如果想撤回该操作可以使用`git reflog`查看所有的分支[查看reflog的完整说明](#查看全部分支提交)

#### 保留本地文件但git不再跟踪

保留本地文件，但让git在下一次提交开始不再跟踪它

```shell
git rm --cached
```

#### 从暂存区恢复到工作区

```shell
git restore <file>
```

### 检查当前文件状态

#### 全量状态

``` shell
git status
```

#### 简略状态

``` shell
git status -s
# git status --short

# 左侧状态为暂存区状态
# 右侧状态为工作区状态

# A 新添加至暂存区
# M 已修改过的文件
# D 已从
# ?? 新增加的未跟踪文件
```

### 提交

#### 提交更新

``` shell
git commit

# 跳过add步骤
git commit -a
```

#### vim常用命令

``` shell
# 进入插入模式
i

# 返回普通模式
esc

# 保存并推出
:wq

# 强制退出不保存
:q!

# 仅保存不推出
:w
```

#### 撤销提交

``` shell
git commit --amend
```

### 推送

#### 将已提交的分支推送到云端分支

``` shell
git push
```

#### 将工作区的文件恢复到最近一次提交状态

``` shell
# 丢弃单个文件的修改
git checkout -- <file>

# 丢弃所有文件的修改
git checkout -- .

# Warning 此操作不可撤销，修改一旦丢弃就无法找回
# 只能恢复未暂存的文件

# 在Git 2.23 后，官方文档更推荐使用以下命令来恢复工作区的文件
git restore
```

#### 移除文件

``` shell
git rm <file>

# 将已工作文件从暂存区移除但仍保留在工作区内
git rm --cached <file>
```

#### 重命名已跟踪文件

``` shell
git mv <oldFile> <newFile>

# 运行此命令相当于运行下面三条命令
mv README.md README
git rm README.md
git add README
```

#### 文件差异对比

``` shell
# 工作区 vs 暂存区
git diff

# 暂存区 vs 最新提交
git diff --staged

# 工作区 vs 最新提交
git diff HEAD

# 查看指定文件的差异
git diff <file>
```

#### 查看日志

``` shell
git log

# 显示每次提交所引入的差异
git log -p
git log --patch

# 显示简略的提交信息
git log -s
git log --stat	

# 单行显示
git log --oneline
```

##### 查看全部分支提交

在我们使用`reset`后，如有需要返回当前HEAD之后的分支，可以使用该命令来查看全部的提交记录

```shell
git reflog
```

### 分支

#### 查看所有分支

``` shell
git branch
# 按q退出
```

#### 创建新分支

``` shell
# 创建并切换分支
git switch -c/-create <branch(分支)>

# 单独创建分支
git branch 分支名
```

#### 删除分支

```shell
git branch -d <branch
```

#### 切换分支

``` shell
# git2.23引入新特性
git switch <branch(分支)>

# 旧版本的切换分支
git checkout <branch>
```

#### 分支合并

```shell
# 切换到要合入的分支
git switch <branch>

# 合并分支
git merge <被合并的分支名>

# 删除合并后不需要的分支指针
git branch -d <branch>
```

> [!NOTE]
>
> 如在合并中产生冲突，需要在VsCode中将冲突解决然后合并

## Git设置

### 修改默认不区分大小写

``` shell
  git config core.ignorecase false
```

### 配置提交时姓名与邮箱

```shell
git config --global user.name "ClearSparkling"

git config --global user.email "clearsparkling@icloud.com"
```

## Git提交记录规范

`feat`:新功能/新特性

`fix`:bug修复

`docs`:文档、README、注释

`style`:修改样式，并不影响代码逻辑

`refactor`:重构代码，功能不变

`perf`:性能优化

`test`:新增或修改测试

`build`:构建、依赖、打包

`ci`:CI/CD配置

`chore`:杂项，如.gitignore、工具配置

`revert`:撤回某个提交

> [!NOTE]
>
> 第一行简述，做了什么，尽可能简洁
>
> 使用动词开头，新增、修复、优化、移除、重构等等
>
> 一次提交只做一类相对独立的事
>
> 不写无意义的信息

具体格式

feat:支持文档历史版本回滚



\- 保存回滚前内容并创建新版本

\- 使用unified diff展示内容差异

\- 限制用户只能回滚自己的文档
