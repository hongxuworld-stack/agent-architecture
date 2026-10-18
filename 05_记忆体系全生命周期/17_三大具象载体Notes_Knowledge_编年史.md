> [📖 返回手册总目录](../README.md) | [← 上一章：16_记忆金字塔全景L1到L4.md](16_记忆金字塔全景L1到L4.md) | [下一章：18_物理存储载体与事件溯源更新哲学.md →](18_物理存储载体与事件溯源更新哲学.md)\n\n---\n\n## 第 17 章：三大核心具象载体深度解析：Notes（便签）、Knowledge（知识）、编年史（Chronicle）

### 17.1 小白入门通俗理解：三种笔记形态的精准定位

面对海量的信息，智能体绝不能把所有东西都存进同一个大文件里，而是要根据**用途、更新频次与稳定性**分化为三大具象载体：
1. **Notes（便签）＝“工作推进手记”**：
   * 记录当前正在攻坚的具体进展，例如：“步骤 1/4 已完成，配置文件已改好，下一步准备执行自动化迁移”；
2. **Knowledge（知识）＝“教训与公理”**：
   * 记录经受过实战检验的长期结论，例如：“不要在 macOS 上直接安装低于 1.20 的特定三方库，必须加 `--no-cache` 参数”；
3. **编年史（Chronicle）＝“大事记长卷”**：
   * 记录版本里程碑，例如：“2026-10-15：全面完成核心架构自愈重构，引入 5 大 Hook 安全门禁”。

---

### 17.2 生产级 Markdown Schema 契约

#### 1. Notes（任务便签规范）
```markdown
# Current Task Notes (活跃任务便签)
- **Goal (当前核心目标)**: 解决跨域请求 403 跨域拦截
- **Active State (当前状态)**: IN_PROGRESS
- **Checkpoints (推进检查点)**:
  - [x] 排查 Nginx 配置中的 CORS 响应头
  - [x] 修复后端 FastAPI 中间件的 allow_origins
  - [ ] 运行端到端集成测试 verify_cors.py
  - [ ] 提交代码并生成变更日志
- **Scratch Observations (临时观察碎片)**:
  - 发现 preflight OPTIONS 请求返回了 200，但实际 POST 被网关重写阻断。
```

#### 2. Knowledge（长期知识项规范）
```markdown
---
id: kn-fastapi-cors-01
created_at: 2026-10-17T12:00:00Z
domain: backend_infra
confidence: 0.95
access_count: 14
tags: [fastapi, nginx, cors]
---
# FastAPI 与 Nginx 双重 CORS 覆盖陷阱
### 现象与根因
当同时在 Nginx 与 FastAPI 配置 `Access-Control-Allow-Origin: *` 时，浏览器会因返回了两个重复的 CORS 头部而直接抛出跨域违规拦截。
### 工业级治理规范
在微服务网关层（Nginx）统一剥离或直接下放给后端，绝对不要在两层同时显式声明通配符头部。
```\n\n---\n\n> [← 上一章：16_记忆金字塔全景L1到L4.md](16_记忆金字塔全景L1到L4.md) | [📖 返回手册总目录](../README.md) | [下一章：18_物理存储载体与事件溯源更新哲学.md →](18_物理存储载体与事件溯源更新哲学.md)\n