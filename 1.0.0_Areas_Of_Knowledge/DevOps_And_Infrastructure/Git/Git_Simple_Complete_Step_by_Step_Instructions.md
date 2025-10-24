# Areas_Of_Knowledge\DevOps_And_Infrastructure\Git\Git_Simple_Complete_Step_by_Step_Instructions.md

这个文件完整的将部分简单的流程说明清楚，而不是告诉用户如何去使用每一行命令；

---

*   **Git操作流程**
    *   [**首次推送**](#首次推送流程)
    *   [**后续推送**](#后续推送流程)
    *   [**拉取仓库**](#拉取仓库流程)

---

# 首次推送流程

这个命令流程应该是完全的首次的推送代码到GitHub仓库中，你在执行这个命令的时候，应该已经创建一个空白的仓库，同时应该在你的GitHub账户中创建一个设备密钥，或者是单独对推送的仓库创建一个密钥；
```PowerShell
git init
```
初始化仓库
```PowerShell
git add .
```
```PowerShell
git commit -m "Initial commit of Forge project"
```
```PowerShell
git remote add origin https://github.com/EldricArlo/Forge.git
```
这条命令用来连接git到github指定的仓库；
这个链接需要替换成你的仓库链接；
```PowerShell
git push -u origin main
```
使用这条命令应该保证你的仓库没有任何的文件和代码；
否则，你需要添加一个新的指令：
```PowerShell
git push -u -f origin main
git push -u origin main -force
git push -uf origin main
```

---

# 后续推送流程

这个操作需要你已经确保已经完成了github仓库的连接；
```PowerShell
git add .
```
```PowerShell
git commit -m "Updated"
```
```PowerShell
git push
```

---

# 拉取仓库流程

当需要将仓库内容替换掉本地内容的时候，执行这个流程；
```PowerShell
git fetch origin
```
```PowerShell
git reset --hard origin/main
```
```PowerShell
git clean -fd
```

