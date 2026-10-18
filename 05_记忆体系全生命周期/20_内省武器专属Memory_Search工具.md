> [📖 返回手册总目录](../README.md) | [← 上一章：19_因果保证_递增ID因果DAG与时间衰减.md](19_因果保证_递增ID因果DAG与时间衰减.md) | [下一章：21_昼夜节律更新白天异步与夜间Cron批处理.md →](21_昼夜节律更新白天异步与夜间Cron批处理.md)\n\n---\n\n## 第 20 章：内省武器：专属 Memory Search 工具（主动翻找 vs 被动联想）

### 20.1 小白入门通俗理解：主动翻找 vs 被动联想

* **被动联想（Passive Injection 傻瓜式无脑灌入）**：  
  就像一个神经质、极度话痨的助理。你每对他说一句话，他就自作聪明地从抽屉里抓出 10 张陈年旧便签硬贴在你眼前。  
  **危害**：哪怕你在谈论今天的天气，他也把三个月前修电闸的笔记塞给你。这不仅迅速吃光你的工作记忆，还会严重干扰你的正常思考（注意力稀释与幻觉）；
* **主动翻找（Active Retrieval 内省工具）**：  
  就像一个冷静有素的专业侦探。平时桌面上干干净净，只留核心线索。当大模型在推演中遇到阻碍（例如：“我记得这个组件曾经有过一个鉴权报错，我需要翻找一下”），**模型自己主动调用专用的 `memory_search` 工具，带着精确的关键词和时间过滤条件去档案库检索！**

---

### 20.2 专属 `memory_search` 工具 Schema 规范

```json
{
  "name": "memory_search",
  "description": "智能体主动内省工具：用于在跨会话的长期记忆与经验知识库中检索历史踩坑记录、技术决策与业务约定",
  "parameters": {
    "type": "object",
    "required": ["query"],
    "properties": {
      "query": {
        "type": "string",
        "description": "检索关键词或语义提问语句，例如 'macOS libmagic 安装报错'"
      },
      "domain_filter": {
        "type": "string",
        "enum": ["all", "infra", "business_logic", "code_style", "incident_postmortem"],
        "description": "领域过滤作用域，用于进一步缩小检索边界"
      },
      "max_results": {
        "type": "integer",
        "default": 3,
        "description": "召回精选记忆项的最大数量，严防上下文溢出"
      }
    }
  }
}
```\n\n---\n\n> [← 上一章：19_因果保证_递增ID因果DAG与时间衰减.md](19_因果保证_递增ID因果DAG与时间衰减.md) | [📖 返回手册总目录](../README.md) | [下一章：21_昼夜节律更新白天异步与夜间Cron批处理.md →](21_昼夜节律更新白天异步与夜间Cron批处理.md)\n