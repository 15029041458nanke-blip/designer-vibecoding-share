# 设计师如何从 0-1 搭 1 套 vibecoding 协作链路

> 分享提炼版  
> 设计驱动 · 多模式协作 · 可模板化

## 01. 封面

这次最重要的收获，不是做出了一个思维导图工具，而是把“设计师怎么和 AI 一起做项目”沉淀成了一套能复用的模板。

**核心判断**

从“我和 AI 聊一聊”，变成“我带着一个 AI 团队做项目”。

---

## 02. 立项

### 先确定 MVP 范围，以验证为目标

0-1 最重要的不是想得全，而是知道什么先不做。

### 为什么做

- 设计稿已经就绪，我对完整方案也足够熟悉
- 文档整理和结构发散这个痛点一直没被解决
- 思维导图刚好能把这个痛点转成一个真实产品

### 为什么先定 MVP 范围

- 我当时并不确定 AI 能不能做出这么复杂的交互产品
- 所以先把范围压成最小闭环，用来验证 vibecoding 的能力
- 先验证“能不能跑通”，再决定要不要继续往深处做

### MVP

- 创建 / 编辑 / 保存
- 拖拽 / 缩放 / 导出
- 先跑通闭环，再谈扩展

> 范围不是压小了，而是被压成了一个可以被验证的 MVP。

---

## 03. 借力启动

### 0-1 不等于从零造轮子

AI 先做研究和技术选型，人再做取舍。下面用这个思维导图项目举例。

### 候选方案

- MindElixir
- React Flow / XYFlow
- Markmap

### 筛选标准

- 交互控制力
- 扩展性
- 导出与状态管理

### 最终技术方案

- React + XYFlow
- Zustand
- dagre
- localforage
- jsPDF

> 技术选型的核心不是“全从零做”，而是“先快速筛出能承载产品方向的底座”。

---

## 04. 借鉴 Agent 配置

### 有了技术底座，下一步是：把别人总结的最佳工作方式也学进来

GitHub 上有不少高星的开发 agent 配置规范，片段式但真实有效。我把其中的核心原则直接落进了自己的 `AGENTS.md`。

### 借来的 4 条原则

- 复杂任务先进 Plan
- 研究和并行分析拆出去
- 每次纠正都写进 lessons
- 完成前必须留下证据

### 借鉴的 Agent 配置原则（节选）

```text
工作流编排（Workflow Orchestration）
1. 默认进入 Plan 模式
- 任何非简单任务（3 步以上或涉及架构决策）都进入 plan 模式
- 一旦出现偏差，立刻停止并重新规划
- plan 模式不仅用于搭建，也用于验证步骤
- 先写详细规格说明（spec）降低歧义

2. 子代理策略（Subagent Strategy）
- 大量使用子代理，让主上下文窗口保持干净
- 研究、探索和并行分析交给子代理处理
- 面对复杂问题，用子代理堆算力
- 每个子代理只负责一条思路

3. 自我改进闭环（Self-Improvement Loop）
- 用户每次纠正后：写入 tasks/lessons.md
- 给自己写规则，避免同类错误再次发生
- 持续迭代这些经验，直到错误率下降
- 每次会话开始时，先回顾 lessons

4. 完成前必须验证（Verification Before Done）
- 没有证明能跑通，就不要标记任务完成
- 必要时对比主分支与修改后的行为差异（diff）
- 自问：一个 Staff Engineer 会认可吗？
- 跑测试、查日志、给出可验证证据

任务管理（Task Management）
- 先写计划：把 plan 写到 tasks/todo.md
- 验证计划：开始实现前先确认/对齐计划
- 跟踪进度：做一项勾一项
- 记录结果：在 tasks/todo.md 增加 review 小节
- 沉淀经验：用户纠正后更新 tasks/lessons.md
```

---

## 05. 模型分工

### 为什么我最早选择 Claude Code + Codex

一个现实前提：公司 CodeWiz 给了 Claude Code 的 tokens 配额，所以我会优先把高上下文的规划工作放在 Claude 侧。

### Claude Code

- 需求澄清
- 上下文管理
- 任务拆解与 handoff

### Codex

- 实现与多文件修改
- 测试与修问题
- 补完整验证链路

### 这条链路解决了什么

- 上下文精简：Claude 收束长上下文，Codex 只接执行必需信息
- 控制成本：高价值规划交给更稳的模型，高频执行交给更适合落地的模型
- 天然带 review：规划和执行拆开之后，模型之间会自然形成校正

---

## 06. 演进

### 协作模式是怎么一步步长成团队的

从两个模型分工，到多 agent 角色化，到加入设计角色。每一步都是被现实问题逼出来的。

### 阶段 1

**Claude Code + Codex 两模型分工**

把“想明白”和“做出来”拆开。Claude 管上下文与规划，Codex 接执行。

### 阶段 2

**Codex 多 agent，执行端角色化**

- `project-manager`：目标收口、范围控制、优先级
- `architect`：技术路径设计、模块拆解
- `engineer`：实现、联调、多文件修改
- `tester`：跑验证、补测试、守关键路径
- `reviewer`：回归风险、质量、交付完整度

### 阶段 3

**Figma MCP + design-analyst，设计角色上线**

- Figma 读稿
- design-analyst 解析
- 组件映射 + token 约束 + 状态定义
- 产出工程可接手的 PRD

### 设计分析的 3 类输出

- 设计稿有、PRD 未提：标注疑点，询问
- PRD 有、设计稿未覆盖：标注风险，可能遗漏
- 一致项：直接写入 AC，工程可直接接手

> 最后追求的不是“一个更强的 AI”，而是“一套分工更顺的小团队”。

---

## 07. 设计转译

### 加入 design-analyst 之后，PRD 变得完全不一样

这页不讲抽象原则，直接看加入设计分析角色前后，需求描述长什么样。

### 没有 design-analyst 时

```text
PRD 节选

目标：
实现首页导航区域

需求：
- 页面风格和 Figma 保持一致
- 顶部有搜索、切换按钮和操作区
- 卡片要有 hover 状态
- 颜色和间距尽量还原

问题：
- 哪些是组件，哪些只是视觉相似，没写清楚
- 状态只有 hover，没有 disabled / loading
- spacing、颜色、字号没有 token 映射
- 工程拿到后仍然要自己猜
```

### 加入 design-analyst 后

```text
PRD 节选

模块：Header / Toolbar

组件映射：
- SearchInput → 使用 Input/Search 组件
- ViewSwitch → SegmentedControl，2 个状态：map / document
- PrimaryAction → Button/Primary

设计约束：
- 间距：gap-16 / padding-x-24
- 字体：title-sm / body-sm
- 颜色：surface-default / text-primary / border-subtle
- 状态：default / hover / active / disabled / loading

验收：
- 所有颜色与间距必须走 tokens
- 图标尺寸固定 20px
- Header 高度 72px，不允许自由漂移
```

> 设计稿不是直接交给开发，而是先被翻译成：组件映射 + token 约束 + 状态定义 + 验收标准。

---

## 08. 产品化

### 最后我把它收成了 2 种开发路径 + 3 条协作模式

其中 Codex 多子 agent 这条链路，并不绑定某个单一模型，它的重点是把角色拆开，而不是把能力绑死。

### 开发路径 1：0-1 无设计稿项目

- 意图理解
- 产品规划
- PRD / spec / handoff
- 开发 / 测试 / 验收

### 开发路径 2：基于设计稿的项目

- 设计分析
- 设计转 PRD
- 产品校验
- 开发 / 测试 / Design QA

### 协作模式

| 协作模式 | 适用场景 | 角色分配 |
| --- | --- | --- |
| Claude + Codex | Claude 还有配额，需求复杂、上下文长 | Claude 管规划，Codex 管执行 |
| Codex 多 agent | 需求较清楚，希望端到端推进；任何单一模型都能按这条拆角色链路来跑 | 重点不是模型名，而是 `product / architect / engineer / tester / reviewer` 的角色拆分 |
| OpenClaw | 希望任务在后台持续工作 | 叠加在前两条模式上，负责执行与回传 |

---

## 09. 阶段性产物

### 阶段性产物：一套跑通了的协作脚手架

把上面所有探索沉淀成文件，做到能复用、能一键初始化。这是那个阶段的交付物，也是下一个问题的起点。

### 核心文件

- `AGENTS.md`：总规则入口。流程、边界、DoD 都从这里开始
- `current-workflow.md`：唯一工作流开关，决定当前到底走哪条主链路
- `handoff.json`：唯一任务源。规划好的任务都从这里进入执行层
- `ai-workflows/`：两条主工作流与共享模板，固化协作模式和角色分工
- `status.json`：唯一状态源。运行进度、阻塞和完成状态都在这里看
- `agent_run.sh`：统一执行入口。本地跑和远程跑，最终都落到这里

---

## 10. 架构挑战·混乱

### 新增一种模板布局方向，发现所有连接线逻辑要重新实现一遍

这是脚手架第一版交付之后遇到的第一个真实打击：以为只是“加个方向选项”，结果打开了一个无法局部修复的混乱。

### 触发事件

用户想新增一种思维导图布局：从默认的“上下展开”改成“左右展开”。

听起来很简单，加个方向参数就行。

结果：导出图片时，所有连接线方向全部错误。

### 现场症状

改了 `EditorPage`，没改 `snapshot`；改了节点位置，没改导线方向；加了参数，5 个地方各自为政，没有一处是对的。

### 发现：同一套“方向”逻辑散落 5 处

```ts
// EditorPage.tsx
if (isLR) sourceHandle = 'source-right'
else      sourceHandle = 'source-bottom'

// MindmapNode.tsx
if (mode==='left-right') position = Right

// snapshot.ts
sourcePosition: Position.Right  // 硬编码

// FoldButton.css / AddButton.css
.fold-btn { left: -1.2rem }  // 写死位置
```

> 新增一种布局，需要改 5 个文件，而且极易漏改。

---

## 11. 架构挑战·优化

### 问题不是 bug，而是代码架构问题

找到根因之后，解法反而很清晰：建一个单一配置文件，其他所有地方都来读它。

### 根因

- 没有明确的架构规范，相同逻辑可以出现在任意文件
- 没有 review 检查点，实现完就算完，没人把关
- 没有“新增功能时检查现有影响”的意识
- AI 只会按要求做，不会主动指出散落的耦合

### 优化：单一数据源 `layoutConfig.ts`

```ts
// layout/layoutConfig.ts ← 唯一改动处
'right-left': {
  sourceHandleId: 'source-left',
  sourcePosition: Position.Left,
  foldButton: { side: 'left' },
}

// 其他所有文件只读配置：
const cfg = getLayoutConfig(mode) // 自动适配
```

### 优化结果

- 改动前：要改 5 个文件
- 改动后：只改 1 个文件

### 沉淀成规则（写进 `AGENTS.md`）

```md
## 架构守护
新需求前快速扫描：
- 相同逻辑是否出现在 2+ 个文件？→ 应抽取
- 数据来源是否唯一（Single Source of Truth）？
- 改动文件是否超过 500 行？→ 应提前拆分
```

> 新增布局，只改 1 个文件，其余全部自动适配。

---

## 12. 架构挑战·转折

### 这次架构混乱让我意识到：缺的不是更好的 AI，而是一套开发规范

我没有系统的开发经验积累，没有清晰的架构守则。AI 只会按要求做，它不会主动告诉你“这样写以后会出问题”。

### 以前的认知

遇到问题 → 问 AI → AI 给一个解法 → 跑通 → 下一个问题

每次 AI 给我的代码，我只知道“能不能跑”，不知道“架构对不对”。

结果是：问题会以不同的形式反复出现。

### 关键认知转变

这不是 AI 能力的问题，是我没有一套规范化的开发框架。

既然我自己没有经验积累，那就去找有经验的人总结好的框架直接用。不是 prompt 技巧，而是完整的开发协作规范。

### 引出下一步

1. 发现架构混乱，意识到缺乏规范
2. 搜索社区实践，找到 Superpowers Skill 集
3. 引入并结合 `design-analyst`，形成最终协作形态

> 一个人 vibecoding，最终需要的是一套完整的团队协作规范，而不是更厉害的 prompt。

---

## 13. Superpowers Skill

### 引入 GitHub 高星开发全家桶 Skill 后，随机子 Agent 进化成了结构化的子 SKILL

Superpowers 是社区沉淀的最佳实践 Skill 集。引入后结合自定义的 `design-analyst` 角色，构成了最终协作形态。最关键的进化，是从“随机调用子 Agent”变成“有规范的子 SKILL”。

### 以前：随机子 Agent（临时指派）

```text
每次临时说“你现在扮演 Reviewer”
提示词散落在对话里，无法复用
没有触发条件，靠记忆决定何时调用
输出格式不固定，每次结果不一样
新会话全部忘记，重新口头约定
```

### 现在：结构化子 SKILL（规范触发）

```text
skills/two-stage-review/SKILL.md
  触发条件：每个 task 完成后
  Phase-1：Spec 合规 Review
  Phase-2：代码质量 Review
  输出：通过/不通过 + 修复清单

skills/design-analysis/SKILL.md
  触发条件：有 Figma 链接输入时
  Phase-1：设计分析包
  Phase-2：Design QA（苛刻还原验收）
```

### 7 个结构化子 SKILL

- `design-analysis`：Figma 读稿 → 设计分析包 → Design QA
- `requirements`：DoR 检查 + AC 补全 → 完整 PRD
- `sys-debugging`：四阶段 Debug，根因必须先找到
- `writing-plans`：No Placeholder 规定，每步必须有验证
- `brainstorming`：需求不清时的探索规范
- `two-stage-review`：Spec 合规 + 代码质量两关门控
- `arch-check`：架构变更前强制扫描，禁止沉默

### 最终协作形态

`design-analysis + requirements + writing-plans + two-stage-review + arch-check`

每个 SKILL 都有：触发条件、执行步骤、输出产物、禁止清单。不靠记忆，一触即发，输出稳定。

**核心判断**

习惯 > Prompt  
SKILL > Agent  
规则写文件 > 对话口述

子 SKILL 是最稳定、最可复用的协作单元。

---

## 14. 总结

### 最后我想沉淀的，不只是经验，而是一个 skill

### Takeaway 1

**先学会控 scope**

0-1 最重要的不是做得全，而是先跑通最小闭环。

### Takeaway 2

**先搭流程，再拼 prompt**

稳定产出靠工作流、角色和验证，不是一句神奇提问。

### Takeaway 3

**把方法做成模板**

最终目标是一个启动即问“有没有设计稿”的设计师 vibecoding skill。

> 分享不是终点。最后留下来的，应该是一套别人也能直接启动的项目模板。

---

## 15. 获取 Skill

### 一条命令，把这套模板带走

复制命令，直接告诉 AI 让他帮你安装。

### 安装命令

```bash
claude skills install github.com/15029041458nanke-blip/designer-vibecoding-starter
```

### 安装完成后的第一步

```text
# 1. 打开任意项目，告诉 Claude：
"帮我搭一套协作脚手架"

# 2. Claude 弹出对话框，回答 4 个问题：
#    - 项目名称
#    - 是否有设计稿
#    - 协作模式
#    - 是否需要 OpenClaw

# 3. 脚手架自动生成，打开 AGENTS.md 开始
```

### 包含什么

- 2 条开发路径（有 / 无设计稿）
- 3 条协作链路（Claude+Codex / Codex 全流程 / OpenClaw）
- 初始化脚本 + 12 个核心文件
- 中文意图切换命令

### 适合谁

- 想用 AI 做项目但不知道从哪搭流程的设计师
- 需要 Claude + Codex 协作的开发者
- 想把项目初始化标准化的团队
