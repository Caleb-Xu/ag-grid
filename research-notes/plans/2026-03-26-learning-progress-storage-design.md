# 学习进度存储拆分设计

## 背景
当前 `research-notes/meta/20-learning-progress.md` 同时承担了三类职责：
- 当前学习状态快照
- 已完成 checkpoint 的历史累积
- 部分长期理解沉淀

随着学习推进，这会让单一 progress 文件持续增长，导致：
- 续课时读取成本越来越高
- 当前状态与历史轨迹混在一起
- 与 `meta/` / `plans/` / `notes/` 的分层边界逐渐模糊

因此需要把“当前快照”和“历史归档”拆开，同时保留 `notes/` 作为长期知识沉淀位置。

## 目标
把学习记录拆成三层职责：
- **快照**：回答“现在学到哪了”
- **历史**：回答“之前经过了哪些 checkpoint”
- **沉淀**：回答“最后稳定留下了什么理解”

## 非目标
- 不改变现有学习主线与专题顺序
- 不把所有历史都迁移进 `notes/`
- 不让 guided learning 启动时默认读取全部历史文件

## 设计概览
保留：
- `research-notes/meta/20-learning-progress.md`
- `research-notes/notes/`

新增：
- `research-notes/meta/history/`
  - 每讲 / 每专题一个 history 文件

建议命名：
- `research-notes/meta/history/01-monorepo-and-packages.md`
- `research-notes/meta/history/02-ag-grid-community-core.md`

命名规则：
- history / notes / roadmap 中涉及同一专题时，尽量共享同一套 topic 编号与 slug
- 由 topic 标题派生文件名时，优先复用现有 `notes/` 或 roadmap 已出现的命名风格，减少人工映射

## 文件职责

### 1. `meta/20-learning-progress.md`
这是**当前快照文件**，只服务于“恢复现场”。

只保留：
- 当前专题
- 当前所处 checkpoint
- 继续当前专题所必需的最小已确认理解
- 下一步
- 最近更新时间

规则：
- 采用**覆盖式更新**
- 不做无限历史追加
- 只保留当前学习继续所需的信息
- 如果某条稳定结论已经沉淀到 `notes/`，这里最多保留一句引用式摘要，或不再重复保留

### 2. `meta/history/<topic>.md`
这是**过程归档文件**，按讲次 / 专题拆开。

每个文件记录：
- 讲次 / 专题名
- 本专题 checkpoint 时间线
- 每次完成了什么 checkpoint
- 每次新增了哪些理解
- 当时下一步是什么

规则：
- 采用**追加式更新**
- 每次 checkpoint 完成时追加一条
- 一个专题的历史只写进自己的 history 文件

### 3. `notes/<topic>.md`
这是**长期知识文件**。

只在专题理解已经比较稳定时写入：
- 结构化架构理解
- 关键源码入口
- facts / inferences / reusable ideas
- 对后续自研 enterprise 有复用价值的结论

规则：
- 不按每个 checkpoint 机械追加
- 以“专题沉淀”为单位整理

## 更新时机

### checkpoint 完成时
同时做两件事：

1. 更新 `20-learning-progress.md`
   - 把当前状态覆盖成最新快照

2. 追加对应的 `meta/history/<topic>.md`
   - 记下这次 checkpoint 的完成记录

一致性规则：
- checkpoint 一旦被确认完成，就必须基于同一份结构化更新内容，同时更新 snapshot 与对应 history
- 如果只完成其中一个文件的更新，则该次 checkpoint 记录视为未完成，结束前需要补齐
- `ag-grid-teaching-aide` 输出 progress 相关建议时，应同时给出 snapshot update 与 history entry 两部分，而不是只给其中之一

### 专题形成稳定理解时
3. 整理到 `notes/<topic>.md`
   - 作为正式长期笔记沉淀

## 读取顺序
继续上课时，主 assistant 默认读取：
1. `research-notes/00-overview.md`
2. `research-notes/plans/02-learning-roadmap-v1.md`
3. `research-notes/meta/10-learning-guide-agent.md`
4. `research-notes/meta/20-learning-progress.md`

不默认全量读取 `meta/history/`。
只有在需要回溯某一讲演进过程时，再定向读取对应 history 文件。

这样可以保证：
- 启动成本低
- 当前上下文清晰
- 历史不会污染快照

## 对 guided learning 机制的影响
后续实现时，需要同步收紧以下规则：
- `20-learning-progress.md` = checkpoint 完成时覆盖快照
- `meta/history/<topic>.md` = checkpoint 完成时追加归档
- `notes/` = 专题稳定后沉淀
- `ag-grid-guided-learning` 启动时默认只读 progress，不全量读 history
- `ag-grid-teaching-aide` 在输出 progress 相关建议时，需要区分：
  - snapshot update
  - history entry

## 预期收益
- 避免单一 progress 文件无限增长
- 保持“恢复现场”和“历史回放”两个任务各自简单
- 让 `meta/` / `history/` / `notes/` 的分工更稳定
- 为后续更多讲次提供可持续的记录结构
