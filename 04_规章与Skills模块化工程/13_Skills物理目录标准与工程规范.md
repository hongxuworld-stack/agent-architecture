> [📖 返回手册总目录](../README.md) | [← 上一章：12_拦截决策矩阵与参数Overwrite重写.md](../03_生命周期Hook安全拦截/12_拦截决策矩阵与参数Overwrite重写.md) | [下一章：14_渐进式披露Progressive_Disclosure加载时序.md →](14_渐进式披露Progressive_Disclosure加载时序.md)\n\n---\n\n## 第 13 章：Skills 的物理目录标准与工程规范：SKILL.md、scripts 与 references

### 13.1 小白入门通俗理解：Tool 与 Skill 的本质差异

许多刚接触智能体开发的同学，最容易混淆的两个概念就是 **Tool（工具）** 与 **Skill（技能）**：
* **Tool（工具）＝ 智能体的“手脚和器械”**：
  * 它只负责提供底层物理操作能力（例如：`view_file` 负责读硬盘，`run_command` 负责调终端）；
  * 工具本身没有任何业务逻辑。它不知道“怎么写一部好短剧”，也不知道“怎么重构遗留代码”，它只是机械地接收参数、执行、返回输出。
* **Skill（技能 / 规章）＝ 智能体大脑里的“专业作业规范（SOP）与专家经验”**：
  * 它是一套高度结构化的指导原则、操作流程、避坑陷阱与专有知识库；
  * 例如当前项目中的 [`commercial-short-drama`](../../.agents/skills/commercial-short-drama/SKILL.md)，就是指导智能体如何一步步检查分镜、严格单视角连贯性、生成多实体 Prompt、规避古典大提琴陷阱的工业级标准作业程序（SOP）。

简单一句话：**Tool 赋予智能体“做事情的能力”，而 Skill 传授智能体“做事情的规矩与手艺”**。

---

### 13.2 工业级 Skills 标准物理目录结构规范

一个生产级 Skill 必须遵循标准化的模块化物理目录标准，严禁散乱堆放：

```text
.agents/skills/<skill-name>/
├── SKILL.md             # 【强制核心】主规章文件 (YAML Frontmatter + 详尽 SOP 说明书)
├── scripts/             # 【可选】辅助执行脚本 (供 Agent 通过 run_command 直接调用的工具箱)
│   ├── build_foley.py   # 例如: 音频自动合成脚本
│   └── check_frames.py  # 例如: 抽帧画面一致性校验器
├── references/          # 【可选】深度参考文档 (冷知识、外部协议文档、复杂参数对照表)
│   └── prompt_rules.md  # 详细提示词规则细则
└── examples/            # 【可选】黄金样本库 (成功用例示范，供模型 Few-shot 临摹)
    └── good_storyboard/ # 优质分镜示例
```

#### `SKILL.md` 物理规范必须满足的三大铁律：
1. **必须包含合规的 YAML Frontmatter**：文件开头必须以 `---` 包裹，显式声明 `name` 与 `description`；
2. **`description` 必须精确声明触发条件**：明确说明“在什么业务场景下激活本技能”，严禁模糊泛化（如“这是一个通用技能”），大模型的意图路由器全靠该字段做命中决策；
3. **内容必须可操作、禁空话**：正文必须包含具体的步骤（Step-by-step）、明确的坏味道（Anti-patterns）以及具体的代码/提示词示范。\n\n---\n\n> [← 上一章：12_拦截决策矩阵与参数Overwrite重写.md](../03_生命周期Hook安全拦截/12_拦截决策矩阵与参数Overwrite重写.md) | [📖 返回手册总目录](../README.md) | [下一章：14_渐进式披露Progressive_Disclosure加载时序.md →](14_渐进式披露Progressive_Disclosure加载时序.md)\n