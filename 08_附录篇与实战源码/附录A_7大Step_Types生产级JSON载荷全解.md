> [📖 返回手册总目录](../README.md) | [← 上一章：25_工业级护城河死循环熔断沙箱与受限解码.md](../07_高可用与前沿演进/25_工业级护城河死循环熔断沙箱与受限解码.md) | [下一章：附录B_完整Minimal_Industrial_Agent源码.md →](附录B_完整Minimal_Industrial_Agent源码.md)

---

## 附录 A：7 大 Step Types 真实生产 JSON 载荷全解

### 1. `USER_INPUT`
```json
{
  "step_index": 1,
  "step_id": "step_usr_01J8G5M",
  "source": "USER_EXPLICIT",
  "type": "USER_INPUT",
  "status": "DONE",
  "created_at": "2026-10-17T08:00:00.120Z",
  "content": "请重构 auth.py 中的 Token 校验逻辑，引入 Redis 缓存并增加单元测试",
  "attachments": [],
  "metadata": {"user_id": "u_8848", "client": "antigravity_ide"}
}
```

### 2. `PLANNER_RESPONSE`
```json
{
  "step_index": 2,
  "step_id": "step_pln_01J8G5N",
  "source": "MODEL",
  "type": "PLANNER_RESPONSE",
  "status": "DONE",
  "created_at": "2026-10-17T08:00:03.450Z",
  "thinking": "用户要求重构 auth.py 并引入 Redis 缓存。当前第一步需要先查看 auth.py 的现行逻辑与依赖情况。",
  "content": "我将分步为您完成重构：首先分析现有鉴权逻辑，随后接入 Redis 缓存层，最后编写并运行单元测试验证。",
  "tool_calls": [
    {
      "id": "call_view_01",
      "type": "function",
      "function": {
        "name": "view_file",
        "arguments": "{\"AbsolutePath\":\"/workspace/src/auth.py\",\"StartLine\":1,\"EndLine\":60}"
      }
    }
  ]
}
```

### 3. `TOOL_USE`
```json
{
  "step_index": 3,
  "step_id": "step_use_01J8G5P",
  "source": "SYSTEM",
  "type": "TOOL_USE",
  "status": "RUNNING",
  "created_at": "2026-10-17T08:00:03.480Z",
  "call_id": "call_view_01",
  "tool_name": "view_file",
  "parameters": {
    "AbsolutePath": "/workspace/src/auth.py",
    "StartLine": 1,
    "EndLine": 60
  }
}
```

### 4. `TOOL_RESULT`
```json
{
  "step_index": 4,
  "step_id": "step_res_01J8G5Q",
  "source": "SYSTEM",
  "type": "TOOL_RESULT",
  "status": "DONE",
  "created_at": "2026-10-17T08:00:03.520Z",
  "call_id": "call_view_01",
  "tool_name": "view_file",
  "exit_code": 0,
  "duration_ms": 40,
  "output": "def verify_token(token: str):\n    user = db.query(User).filter_by(token=token).first()\n    return user",
  "error": null
}
```

### 5. `INTERACTION_REQUEST`
```json
{
  "step_index": 5,
  "step_id": "step_itr_01J8G5R",
  "source": "MODEL",
  "type": "INTERACTION_REQUEST",
  "status": "WAITING_FOR_INPUT",
  "created_at": "2026-10-17T08:00:05.100Z",
  "interaction_type": "ask_question",
  "payload": {
    "question": "检测到系统中存在两个 Redis 实例配置，请选择本次重构接入的目标缓存：",
    "options": [
      "(Recommended) 接入独立鉴权集群 redis://auth-cluster.internal:6379",
      "接入全局共享缓存 redis://shared-cache.internal:6379"
    ]
  }
}
```

### 6. `SYSTEM_EVENT`
```json
{
  "step_index": 6,
  "step_id": "step_sys_01J8G5S",
  "source": "SYSTEM",
  "type": "SYSTEM_EVENT",
  "status": "DONE",
  "created_at": "2026-10-17T08:00:15.800Z",
  "event_name": "BACKGROUND_TASK_COMPLETED",
  "payload": {
    "task_id": "task_daemon_pytest_09",
    "exit_code": 0,
    "stdout": "5 passed in 1.45s"
  }
}
```

### 7. `SUBAGENT_DISPATCH`
```json
{
  "step_index": 7,
  "step_id": "step_sub_01J8G5T",
  "source": "MODEL",
  "type": "SUBAGENT_DISPATCH",
  "status": "DONE",
  "created_at": "2026-10-17T08:00:16.200Z",
  "conversation_id": "sub_conv_8848a",
  "subagent_type": "research",
  "role": "Database Benchmark Analyst",
  "prompt": "请并发测试当前 Redis 实例在 1000 QPS 下的读取延迟，并生成分析报表",
  "workspace_mode": "branch"
}
```

---

> [← 上一章：25_工业级护城河死循环熔断沙箱与受限解码.md](../07_高可用与前沿演进/25_工业级护城河死循环熔断沙箱与受限解码.md) | [📖 返回手册总目录](../README.md) | [下一章：附录B_完整Minimal_Industrial_Agent源码.md →](附录B_完整Minimal_Industrial_Agent源码.md)
