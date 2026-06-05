# Git

## Git基础命令

#### 初始化git仓库

``` shell
cd "/url"
git init
```

#### 克隆现有仓库

``` shell
git clone <url>
```

### 添加远程仓库

``` shell
git remote add <shortname> <url>	
```



### 暂存

#### 暂存文件

``` shell
git add <file>
```

#### 取消暂存文件

``` shell
git reset HEAD<file>
```



#### 检查当前文件状态

##### 全量状态

``` shell
git status
```

##### 简略状态

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

#### 撤销提交

``` shell
git commit --amend
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

#### 查看提交历史

``` shell
git log

# 显示每次提交所引入的差异
git log -p
git log --patch

# 显示简略的提交信息
git log -s
git log --stat	
```

### 分支

#### 创建新分支

``` shell
git switch -c/-create <branch(分支)>
```

#### 切换分支

``` shell
# git2.23引入新特性
git switch <branch(分支)>
```
