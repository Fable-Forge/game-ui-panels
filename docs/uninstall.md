# Uninstall game-ui-panels

仅删除用户明确选择的运行时目录中的 `game-ui-panels`：

- `~/.codex/skills/game-ui-panels/`
- `~/.claude/skills/game-ui-panels/`
- `~/.agents/skills/game-ui-panels/`

删除前必须解析并显示绝对路径，确认目录名恰好为 `game-ui-panels`，且位于对应的 `skills` 根目录内。不要递归删除未验证的变量、用户主目录或整个 skills 根目录。存在本地修改时，先让用户决定是否备份。
