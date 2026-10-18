> [📖 返回手册总目录](../README.md) | [← 上一章：附录A_7大Step_Types生产级JSON载荷全解.md](附录A_7大Step_Types生产级JSON载荷全解.md) | [下一章：附录C_生产级hooks_json与安全脚本配置.md →](附录C_生产级hooks_json与安全脚本配置.md)

---

## 附录 B：完整可运行的 Minimal Industrial Agent 源码（Python 原型）

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# Minimal Industrial Agent Core Engine (Python 3.9+)

import os
import sys
import json
import subprocess
from typing import Dict, Any, List, Optional

class IndustrialToolEngine:
    def __init__(self, workspace_root: str):
        self.workspace_root = os.path.abspath(workspace_root)

    def _resolve_safe_path(self, path: str) -> str:
        abs_path = os.path.abspath(os.path.join(self.workspace_root, path))
        if not abs_path.startswith(self.workspace_root):
            raise PermissionError(f"Path traversal detected: {path} is outside workspace!")
        return abs_path

    def view_file(self, AbsolutePath: str, StartLine: int = 1, EndLine: int = 100) -> str:
        safe_path = self._resolve_safe_path(AbsolutePath)
        if not os.path.isfile(safe_path):
            return f"Error: File '{AbsolutePath}' does not exist."
        with open(safe_path, "r", encoding="utf-8", errors="replace") as f:
            lines = f.readlines()
        total_lines = len(lines)
        start = max(1, StartLine) - 1
        end = min(total_lines, EndLine)
        sliced = lines[start:end]
        return f"[{AbsolutePath} lines {start+1}-{end} of {total_lines}]\n" + "".join(sliced)

    def replace_file_content(self, TargetFile: str, TargetContent: str, ReplacementContent: str) -> str:
        safe_path = self._resolve_safe_path(TargetFile)
        if not os.path.isfile(safe_path):
            return f"Error: File '{TargetFile}' not found."
        with open(safe_path, "r", encoding="utf-8") as f:
            content = f.read()
        count = content.count(TargetContent)
        if count == 0:
            return "Error: TargetContent not found in file."
        if count > 1:
            return f"Error: TargetContent found {count} times! Ambiguous replacement prohibited."
        new_content = content.replace(TargetContent, ReplacementContent, 1)
        with open(safe_path, "w", encoding="utf-8") as f:
            f.write(new_content)
        return "Success: 1 chunk replaced atomically."

    def run_command(self, CommandLine: str, Cwd: Optional[str] = None) -> Dict[str, Any]:
        work_dir = self._resolve_safe_path(Cwd) if Cwd else self.workspace_root
        if CommandLine.strip().startswith("cd "):
            return {"exit_code": 1, "output": "", "error": "Error: 'cd' command is forbidden. Use Cwd parameter."}
        try:
            res = subprocess.run(CommandLine, shell=True, cwd=work_dir, capture_output=True, text=True, timeout=30)
            return {"exit_code": res.returncode, "output": res.stdout, "error": res.stderr}
        except subprocess.TimeoutExpired:
            return {"exit_code": 124, "output": "", "error": "Command timed out after 30 seconds."}

class LifecycleHookManager:
    def evaluate_pre_tool_use(self, tool_name: str, parameters: Dict[str, Any]) -> Dict[str, Any]:
        if tool_name == "run_command":
            cmd = parameters.get("CommandLine", "")
            if any(danger in cmd for danger in ["rm -rf /", "mkfs", "> /dev/sda"]):
                return {"decision": "deny", "reason": "CRITICAL: Malicious system destruction command blocked!"}
            if "git push" in cmd and "--force" in cmd:
                new_cmd = cmd.replace("--force", "--force-with-lease")
                return {"decision": "allow", "overwrite": {"CommandLine": new_cmd}, "reason": "Auto safe rewrite"}
        return {"decision": "allow"}

class MinimalIndustrialAgent:
    def __init__(self, workspace: str):
        self.tools = IndustrialToolEngine(workspace)
        self.hooks = LifecycleHookManager()

    def dispatch_tool(self, tool_name: str, params: Dict[str, Any]) -> str:
        hook_res = self.hooks.evaluate_pre_tool_use(tool_name, params)
        if hook_res.get("decision") == "deny":
            return f"[Hook Interception Blocked]: {hook_res.get('reason')}"
        if hook_res.get("overwrite"):
            params.update(hook_res["overwrite"])

        if tool_name == "view_file":
            return self.tools.view_file(params.get("AbsolutePath", ""), params.get("StartLine", 1), params.get("EndLine", 100))
        elif tool_name == "replace_file_content":
            return self.tools.replace_file_content(params.get("TargetFile", ""), params.get("TargetContent", ""), params.get("ReplacementContent", ""))
        elif tool_name == "run_command":
            res = self.tools.run_command(params.get("CommandLine", ""), params.get("Cwd"))
            return f"ExitCode: {res['exit_code']}\nStdout: {res['output']}\nStderr: {res['error']}"
        return f"Unknown tool: {tool_name}"

if __name__ == "__main__":
    agent = MinimalIndustrialAgent(workspace="./")
    print("[*] Minimal Industrial Agent Engine Ready.")
```

---

> [← 上一章：附录A_7大Step_Types生产级JSON载荷全解.md](附录A_7大Step_Types生产级JSON载荷全解.md) | [📖 返回手册总目录](../README.md) | [下一章：附录C_生产级hooks_json与安全脚本配置.md →](附录C_生产级hooks_json与安全脚本配置.md)
