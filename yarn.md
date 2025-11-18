# 推荐方式：使用 Corepack 安装 Yarn

在 Windows PowerShell 执行：

```bash
corepack enable
corepack prepare yarn@1.22.22 --activate
```

# 让 nvm 在所有 zsh 会话中自动加载

执行：

`nano ~/.zshrc`

确认这两段 **一定存在**：

`export NVM_DIR="$HOME/.nvm" [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"`

如果没有，请手动加上。

保存退出（Ctrl+O 回车 / Ctrl+X）。

然后让配置生效：

`source ~/.zshrc`

再试：

`yarn -V`

---

# 🔍 为什么会这样？

因为 **nvm 是脚本加载的，不是系统级安装**。

只要某些 zsh session 没加载 nvm，就会：

- 没有 node
    
- 没有 npm
    
- 也没有 yarn（Corepack 在 Node 里）
    

你刚好就是这个情况。

---

# 🧪 检查一下当前会话有没有 Node：

运行：

`node -v`

如果也提示：

`command not found`