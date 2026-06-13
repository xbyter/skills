---
name: git-guardrails-claude-code
description: 设置 Claude Code 钩子，在执行危险的 git 命令（push、reset --hard、clean、branch -D 等）前拦截它们。当用户想要防止破坏性 git 操作、添加 git 安全钩子或拦截 Claude Code 中的 git push/reset 时使用。
---

# 设置 Git Guardrails

设置 PreToolUse hook，在 Claude 执行前拦截并阻止危险的 git 命令。

## 被阻止的命令

- `git push`（所有变体包括 `--force`）
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

被阻止时，Claude 看到一条消息告知它无权访问这些命令。

## 步骤

### 1. 询问范围

问用户：安装为**仅此项目**（`.claude/settings.json`）还是**所有项目**（`~/.claude/settings.json`）？

### 2. 复制 hook 脚本

捆绑脚本位于：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

根据范围复制到目标位置：

- **Project**: `.claude/hooks/block-dangerous-git.sh`
- **Global**: `~/.claude/hooks/block-dangerous-git.sh`

用 `chmod +x` 使其可执行。

### 3. 添加 hook 到设置

添加到相应的设置文件：

**Project**（`.claude/settings.json`）:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

**Global**（`~/.claude/settings.json`）:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

如果设置文件已存在，将 hook 合并到现有的 `hooks.PreToolUse` 数组中——不要覆盖其他设置。

### 4. 询问自定义

询问用户是否想从阻止列表中添加或删除任何模式。相应编辑复制的脚本。

### 5. 验证

运行快速测试：

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>
```

应以退出码 2 退出并将 BLOCKED 消息打印到 stderr。
