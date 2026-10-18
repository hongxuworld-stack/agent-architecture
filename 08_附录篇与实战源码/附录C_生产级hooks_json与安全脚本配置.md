> [📖 返回手册总目录](../README.md) | [← 上一章：附录B_完整Minimal_Industrial_Agent源码.md](附录B_完整Minimal_Industrial_Agent源码.md) | [下一章：附录D_生产级System_Prompt工程模版.md →](附录D_生产级System_Prompt工程模版.md)

---

## 附录 C：生产级 `hooks.json` 与安全拦截脚本完整配置

```json
{
  "$schema": "https://json.antigravity.dev/v1/hooks.schema.json",
  "version": "1.0",
  "rules": [
    {
      "id": "sec-001-protect-git-history",
      "trigger": "PreToolUse",
      "matcher": {
        "tool_name": "run_command",
        "parameters": {
          "CommandLine": "git\\s+push\\s+.*--force(?!--with-lease)"
        }
      },
      "action": "deny",
      "reason": "Force push without lease is strictly prohibited on all team repositories."
    }
  ]
}
```

---

> [← 上一章：附录B_完整Minimal_Industrial_Agent源码.md](附录B_完整Minimal_Industrial_Agent源码.md) | [📖 返回手册总目录](../README.md) | [下一章：附录D_生产级System_Prompt工程模版.md →](附录D_生产级System_Prompt工程模版.md)
