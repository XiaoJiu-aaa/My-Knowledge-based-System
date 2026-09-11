---
tags:
  - Git
  - 教程
  - 速查
  - 版本控制
created: 2026-09-07
updated: 2026-09-07
---

# Git 学习笔记与命令速查


---

## 一、Git 是什么？为什么需要它？

**Git 是一个版本控制系统（VCS）**，用来记录文件随时间的变化，核心价值：

1. **存档点**：随时可以回到任何历史版本，改崩了也不怕。
2. **分支**：并行开发不同功能，互不干扰，最后合并。
3. **协作**：多人/多设备改同一份代码，靠它合并冲突。
4. **云同步**：配合 GitHub 等平台，代码存云端，换电脑随时拉。

一句话理解：**Git = 给文件打"存档点" + 记录"谁在什么时候改了哪一行"。**

---

## 二、核心概念

| 概念             | 是什么                 | 相关命令                    |
| -------------- | ------------------- | ----------------------- |
| **工作区**        | 你电脑上正在编辑的目录         | —                       |
| **暂存区（index）** | 待提交改动的"暂存台"         | `git add`               |
| **本地仓库**       | 本机保存的历史版本库          | `git commit`            |
| **远程仓库**       | GitHub 等平台上的那份，多人共享 | `git push` / `git pull` |

```
工作区 --git add--> 暂存区 --git commit--> 本地仓库 --git push--> 远程仓库
                                                              ↑
                            别人/别电脑 <--git pull--  远程仓库
```

**关键心态：**
- `add` = 告诉 git「这些改动我要打包」。
- `commit` = 真正打包存档（生成一个不可变的历史节点）。
- 每次 commit 都有唯一的 `哈希值`（如 `4c776d3`）和说明文字。

---

## 三、安装与首次配置

### 安装

- **Windows**：`winget install --id Git.Git`，或到 `git-scm.com/download/win` 下载。
- **macOS**：`brew install git`（已内置，升级用 brew）。
- **Linux**：`sudo apt install git`（Ubuntu/Debian）。

### 首次配置（新设备必做，决定提交署名）

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --global init.defaultBranch main   # 默认主分支名用 main
git config --global pull.rebase true           # pull 用 rebase，历史更干净
git config --list                              # 查看所有配置
```

> `--global` 表示本机所有仓库共用；不加则只对当前仓库生效。

### 帮助

```bash
git help 命令名      # 打开详细文档
git 命令名 --help
```

---

## 四、入门流程（本地单人使用）

```bash
# 1. 初始化仓库（在项目根目录）
git init

# 2. 查看状态（最常用的命令，随时看）
git status

# 3. 把所有改动加入暂存区
git add .

# 4. 提交存档
git commit -m "描述这次改了什么"

# 5. 重复 2~4 循环：改代码 → add → commit
```

**好习惯**：`commit -m` 写得具体，如 `"修复登录页按钮点击无响应"`，别写 `"update"`。

### 查看历史

```bash
git log                      # 完整历史
git log --oneline            # 精简版（推荐）
git log --oneline -5         # 最近 5 条
git log --graph --all        # 带分支关系的图形视图
```

---

## 五、分支（并行开发的利器）

**分支 = 一条独立的开发线。** `main` 是主线，开发新功能时另开分支，做完合并回主线。

```bash
git branch                 # 查看所有分支（* 表示当前分支）
git branch 功能名           # 新建分支
git switch 分支名           # 切换分支（旧命令：git checkout 分支名）
git switch -c 功能名        # 新建并立即切换（最常用）
git branch -d 分支名        # 删除已合并的分支
git branch -D 分支名        # 强制删除未合并的分支
```

### 合并分支

```bash
# 先切到要"接收"的分支，再执行 merge
git switch main
git merge 功能名            # 把功能分支合进 main
```

### 典型协作流程

```bash
git switch -c feature/新功能   # 从当前分支开一条新线
# ……开发、多次 add + commit……
git switch main               # 切回主线
git merge feature/新功能       # 合并
git branch -d feature/新功能   # 合并完删掉旧分支
```

> 提示：分支开发建议**经常 commit**，把每一步都变成可回退的存档点，而不是憋一个大改动。

---

## 六、远程仓库（配合 GitHub）

### 关联远程

```bash
git remote add origin <仓库HTTPS地址>   # origin 是给远程起的默认别名
git remote -v                          # 查看关联了哪些远程
git remote remove origin               # 移除远程关联
git remote set-url origin <新地址>      # 更换远程地址
```

### 克隆（把远程仓库复制到本机）

```bash
git clone <仓库地址>            # 在当前位置创建同名文件夹
git clone <仓库地址> 自定义目录   # 指定克隆目录名
```

### 推送 / 拉取

```bash
git push -u origin main    # 首次推送并建立"上游追踪"（-u = --set-upstream）
git push                   # 之后直接推送
git pull                   # 拉取远程新改动并合并到当前分支
git fetch                  # 只下载远程改动，不自动合并（更谨慎）
```

**`git push` 不写参数为什么知道推哪？**
Git 会查两层记录（都存于仓库本地 `.git/config`）：
1. 当前分支的上游：`branch "main"` → `remote = origin`
2. origin 的真实地址：`remote "origin"` → `url = ...`
`git branch -vv` 可查看分支上游关系。

### 多人/多设备协作要点

```bash
开工前： git pull                       # 先拿到别人的改动
干完活： git add . && git commit -m "x" && git push
```

- 别人先推了新提交、你后推会被拒（non-fast-forward）→ 先 `git pull` 再 `git push`。
- **凭据**：首次 push 时 Git 凭据管理器会弹浏览器登录，登录一次后本机缓存，之后免密。

### git pull 会不会覆盖我的本地内容？

**默认不会。** 它有明确的自我保护：

- **未提交的改动**：如果 pull 要更新的文件正好被你本地改过且未 commit，git 会**报错中止**，绝不覆盖：
  ```
  error: Your local changes to the following files would be overwritten by merge
  ```
  此时先 `git commit` 存档，或 `git stash` 藏起来，再 pull 即可。
- **已提交但没推送的提交**：pull 会把远程改动和你本地提交**合并**（配了 rebase 则是叠放），**不会删掉你的提交**。

真正可能"丢内容"的是另外三条命令：`git reset --hard`、`git clean -f`、乱用 `git push --force`。而 pull 最大的"风险"只是**合并冲突**——冲突时 git 会停下让你手工选择，双方内容并排展示（`<<<<<<<` 标记），由你决定保留哪边。

> **省心习惯**：pull 前若手上有没弄完的改动，先 `git commit` 存档或 `git stash` 藏起来，就万无一失了。

---

## 七、撤销与后悔药

### 还没提交的改动（工作区）

```bash
git restore 文件名          # 丢弃工作区改动，恢复成上次提交的样子
git restore .              # 丢弃当前目录所有未提交改动
git clean -fd              # 删除所有未跟踪的新文件（危险）
```

### 已 add 未 commit

```bash
git restore --staged 文件名   # 把文件从暂存区退回工作区（内容不丢）
git reset HEAD 文件名         # 效果同上（老写法）
```

### 已 commit 未 push

```bash
git reset HEAD~1            # 撤销最近一次提交，改动保留在工作区（推荐）
git reset --soft HEAD~1     # 撤销提交，改动保留在暂存区
git reset --hard HEAD~1     # 撤销提交并彻底丢弃改动（危险！）
git commit --amend -m "新说明"  # 修改上一次提交的说明 / 补交漏掉的文件
```

### 已 push（想改远程历史）

```bash
# 一般不推荐改写已推送的历史；确需回退：
git reset --hard <想回到的commit号>
git push --force-with-lease origin main   # 加 --force-with-lease 更安全
```

**⚠️ 安全准则**：`reset --hard` / `clean -f` / `force push` 会丢改动，使用前确认。

### 还没 commit 想临时切分支（stash）

```bash
git stash              # 把未提交改动暂时藏起来
git stash pop          # 取回最近一次藏起来的改动
git stash list         # 查看藏了什么
git stash drop         # 丢弃藏起来的改动
```

---

## 八、实用技巧

### .gitignore（忽略不想提交的文件）

在项目根目录建 `.gitignore` 文件，每行一条规则：

```gitignore
# 注释用 #
node_modules/
*.log
.env
build/
.DS_Store
```

```bash
git check-ignore -v 文件名    # 查某文件为何被忽略
git rm --cached 文件名        # 让已跟踪的文件停止跟踪（本地文件保留）
git status --ignored         # 查看被忽略的文件
```

### Rebase vs Merge

- `git merge`：把分支合并，保留完整历史，会多出合并节点。
- `git rebase`：把一个分支的提交"叠放"到另一个分支顶端，历史是一条直线，更干净。
- 对新人：**日常先用 merge**，`pull.rebase` 那条配置只影响 pull 时的行为，安全。

### 查看某行是谁写的（找责任人）

```bash
git blame 文件名
git log -p 文件名            # 看某文件每次改动细节
git show <commit号>          # 看某次提交改了什么
```

### 给版本打标签

```bash
git tag v1.0.0              # 给当前提交打标签（发布版本用）
git tag                    # 列出所有标签
git push origin v1.0.0      # 推送标签
```

### 常用别名（偷懒利器）

```bash
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
# 之后 git st = git status，git lg = 图形历史
```

---

## 九、命令速查总表（一页纸）

| 目的 | 命令 |
|------|------|
| 初始化仓库 | `git init` |
| 克隆 | `git clone <地址>` |
| 看状态 | `git status` |
| 暂存改动 | `git add 文件` / `git add .` |
| 提交 | `git commit -m "说明"` |
| 看历史 | `git log --oneline` |
| 看改动 | `git diff` |
| 建/切分支 | `git switch -c 名字` |
| 合并分支 | `git merge 名字` |
| 删分支 | `git branch -d 名字` |
| 推送到远程 | `git push` |
| 拉取远程 | `git pull` |
| 放弃工作区改动 | `git restore 文件` |
| 撤销最近提交 | `git reset HEAD~1` |
| 临时藏改动 | `git stash` |
| 找回改动 | `git stash pop` |
| 忽略文件 | 编辑 `.gitignore` |

---

## 十、常见报错与处理

| 现象 | 原因 | 解决 |
|------|------|------|
| `fatal: not a git repository` | 不在仓库目录 | `cd` 进仓库或用 `git -C 路径` |
| `failed to push some refs` | 远程有你的本地没有的提交 | `git pull` 后再 push |
| 分支没有 upstream | 新分支没设上游 | `git push -u origin 分支名` |
| push 被拒 non-fast-forward | 远程领先本地 | 先 `git pull`（见冲突处理） |
| 合并冲突 `<<<<<<< ======= >>>>>>>` | 两人改了同一处 | 手工取舍 → `git add` → `git commit` |
| `command not found: git` | 装完没刷新 PATH | 重开终端 |
| TLS / 网络超时 | 网络连不上 GitHub | 开代理/加速器后重试 |

### 冲突解决三步

1. 打开冲突文件，删除标记行，保留想要的内容：
   ```
   <<<<<<< HEAD
   我写的
   =======
   别人写的
   >>>>>>> 功能分支
   ```
   改成只剩一处正确内容。
2. `git add 该文件`（标记已解决）
3. `git commit`（完成合并）

---

## 十一、学习路径建议

1. **先练熟**：init / status / add / commit / log —— 本地存档先用顺。
2. **再学**：分支 + merge —— 单人开分支体验并行线。
3. **然后**：remote / clone / push / pull —— 上 GitHub 同步与协作。
4. **最后**：reset 系列 + rebase + 冲突处理 —— 掌握"后悔药"和复杂协作。

> 遇到不懂的报错，先读报错原文（git 提示通常很友好，常直接告诉你该敲什么），再查命令速查表。
