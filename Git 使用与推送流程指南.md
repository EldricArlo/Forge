本文档根据提供的笔记重新整理，旨在为 Git 用户提供一个从初始配置到日常代码推送，再到解决常见问题的完整流程。

---

### 第一部分：初次使用 Git 的全局配置

在开始使用 Git 之前，建议先完成以下一次性配置。这些设置为全局（`--global`）配置，意味着它们将应用于您计算机上的所有 Git 仓库。

#### 1. 设置用户身份

Git 会在每次提交时记录作者信息，所以你需要设置你的用户名和邮箱。

```powershell
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

#### 2. （可选）为 Git 配置网络代理

如果你的网络环境需要通过代理才能访问 GitHub，可以使用以下命令进行配置。

**设置代理：**
```powershell
# 为 HTTP 和 HTTPS 协议设置代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

**验证代理配置：**
```powershell
git config --global --get http.proxy
git config --global --get https.proxy
```
*如果配置成功，上述命令会返回你设置的代理地址。*

**取消代理设置：**
```powershell
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

### 第二部分：使用 SSH 协议连接 GitHub（推荐）

相比于 HTTPS，SSH 是一种更稳定、更安全的连接方式，可以避免许多因网络问题导致的连接失败。强烈建议切换到 SSH 协议。

#### 步骤 1：检查本地是否存在 SSH 密钥

```powershell
# 在 PowerShell 中运行
Get-ChildItem ~/.ssh
```
*如果你在输出中看到 `id_ed25519.pub` 或 `id_rsa.pub` 等文件，说明密钥已经存在，可以跳到步骤 3。*

#### 步骤 2：生成新的 SSH 密钥

如果不存在密钥，请使用以下命令生成。

```powershell
# 将邮箱替换为你自己的 GitHub 邮箱
ssh-keygen -t ed25519 -C "your_email@example.com"
```
生成过程中，系统会询问你：
1.  **保存位置**：直接按回车键使用默认路径。
2.  **输入密码 (passphrase)**：直接按两次回车键设置为空密码，方便日常使用。

#### 步骤 3：将 SSH 公钥添加到 GitHub 账户

1.  **复制公钥内容**：
    ```powershell
    Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
    ```
2.  **添加到 GitHub**：
    *   登录 GitHub，进入 "[SSH and GPG keys](https://github.com/settings/keys)" 页面。
    *   点击 "New SSH key"。
    *   在 "Title" 中为此密钥取一个名字（例如 "My Windows Laptop"）。
    *   在 "Key" 字段中粘贴你刚刚复制的公钥内容。
    *   点击 "Add SSH key"。

#### 步骤 4：测试 SSH 连接

```powershell
ssh -T git@github.com
```
*首次连接时，系统会询问你是否信任该主机，输入 `yes` 并按回车。如果连接成功，你会看到一条包含你的 GitHub 用户名的欢迎信息。*

---

### 第三部分：日常代码提交与推送流程

这是最常用的工作流程，用于将本地的代码变更同步到 GitHub。

#### 步骤 1：暂存文件 (`git add`)

此命令将你的文件更改添加到 Git 的暂存区。

```powershell
# 暂存所有已修改和新建的文件
git add .

# 或者只暂存指定文件
git add <文件名>
```

#### 步骤 2：提交更改 (`git commit`)

将暂存区的内容创建一个新的提交记录，并附带一条说明信息。

```powershell
git commit -m "这里写下本次提交的简要说明"
```
*清晰的提交信息有助于追踪项目历史。*

#### 步骤 3：推送至远程仓库 (`git push`)

将本地的提交记录上传到 GitHub。

```powershell
# 通常，主分支名为 main
git push origin main
```

#### 辅助检查命令

*   **检查当前状态**：查看哪些文件被修改、暂存。
    ```powershell
    git status
    ```
*   **检查远程仓库地址**：确认推送的目标地址是否正确。
    ```powershell
    git remote -v
    ```

---

### 第四部分：项目初始化与首次推送

当你有一个本地项目，想要推送到一个全新的 GitHub 仓库时，请遵循此流程。

#### 步骤 1：在本地项目目录中初始化 Git

```powershell
# 这会在当前目录创建一个 .git 文件夹
git init
```

#### 步骤 2：将本地仓库与远程仓库关联

首先，在 GitHub 上创建一个 **空的** 新仓库（不要勾选任何初始化选项）。然后复制其 SSH 格式的 URL。

```powershell
# 将 URL 替换为你自己的仓库地址
git remote add origin git@github.com:YourUsername/YourRepo.git
```
*如果你已有一个使用 HTTPS 的远程地址，想切换为 SSH，请使用以下命令：*
```powershell
git remote set-url origin git@github.com:YourUsername/YourRepo.git
```

#### 步骤 3：完成首次提交和推送

```powershell
# (可选但推荐) 确保你的主分支名为 main
git branch -M main

# 将所有文件添加到暂存区
git add .

# 创建第一个提交
git commit -m "Initial commit"

# 推送代码，-u 参数会将本地 main 分支与远程 origin/main 分支关联
git push -u origin main
```

---

### 第五部分：同步远程更新与解决冲突

#### 1. 拉取远程更新

当远程仓库有你本地没有的更新时，需要先拉取再推送。

```powershell
# 拉取远程 main 分支的更改并与本地合并
git pull origin main
```
* **另一种合并策略（Rebase）**：`git pull --rebase origin main` 会让提交历史更整洁（呈线性），但解决冲突时对新手可能更复杂。*

#### 2. 解决合并冲突 (Merge Conflict)

如果在 `git pull` 过程中出现冲突，Git 会暂停合并并提示你。

1.  **查看冲突文件**：
    ```powershell
    git status
    ```
    *Git 会在 `Unmerged paths` 部分列出冲突的文件。*

2.  **手动解决冲突**：
    打开冲突文件，你会看到类似 `<<<<<<<`, `=======`, `>>>>>>>` 的标记。手动编辑文件，保留你想要的内容，并删除这些标记。

3.  **（或者）选择一方的版本覆盖**：
    如果你确定要完全保留你本地的版本，可以运行：
    ```powershell
    # --ours 指的是你当前分支的版本
    git checkout --ours <冲突文件名>
    ```

4.  **标记冲突已解决**：
    解决完所有冲突后，将文件重新暂存。
    ```powershell
    git add <已解决的文件名>
    # 如果解决了多个文件
    git add .
    ```

5.  **完成合并**：
    ```powershell
    git commit
    ```
    *此时会弹出一个编辑器让你填写合并信息，直接保存并退出即可。*

6.  **如果搞砸了，想放弃本次合并**：
    ```powershell
    git merge --abort
    ```
    *此命令会让你的仓库回到 `git pull` 之前的状态。*

---

### 第六部分：高级与危险操作

**警告：** 以下操作具有破坏性，请在完全理解其后果后谨慎使用。

#### 1. 强制推送 (`git push --force`)

此命令会用你本地的提交历史强行覆盖远程仓库的历史。**这会删除远程仓库上别人可能已经推送的提交**。仅在确定远程仓库内容无用时才可使用。

```powershell
git push --force origin main
```

#### 2. 清理本地仓库历史

如果你想让一个项目变回一个普通文件夹（删除所有 Git 历史），可以删除其 `.git` 文件夹。

```powershell
# 这个命令会强制递归删除 .git 文件夹
Remove-Item -Recurse -Force .git
```

#### 3. 清理远程 GitHub 仓库

最直接的方法是在 GitHub 网站上，进入仓库的 `Settings` -> `Danger Zone`，然后删除整个仓库。之后可以重新创建一个同名的空仓库。

#### 4. 删除本地 SSH 密钥文件

如果需要删除已生成的密钥（私钥 `id_ed25519` 和公钥 `id_ed25519.pub`），可使用以下命令。

```powershell
Remove-Item ~/.ssh/id_ed25519
Remove-Item ~/.ssh/id_ed25519.pub
```

---

# 在已经推送过仓库之后，推送另一个仓库内容

1. 添加一个新的远程仓库

```Bash
git remote add [new-remote-name] [new-repository-url]
```

例如：

```Bash
git remote add new-origin https://github.com/user/new-repo.git
```

2. 查看远程仓库是否添加成功

```Bash
git remote -v
```

3. 将需要指定新的远程仓库的名称和想要推送的分支名称

```Bash
git push [new-remote-name] [branch-name]
```

例如：

```Bash
git push new-origin main
```

## 如果你想更改现有的远程仓库地址

如果你只是想更改现有远程仓库的 URL，而不是添加一个新的，你可以使用 git remote set-url 命令

例如，如果你想更改名为 origin 的远程仓库的 URL，可以运行

```Bash
git remote set-url origin [new-repository-url]
```

然后，你可以像平常一样使用 git push 将代码推送到新的 URL

# 完整流程

```Bash
git add .
git commit -m "Initial commit"

git branch

git push Froge main

```

## 合并内容

```Bash
git pull Froge main --allow-unrelated-histories
```
