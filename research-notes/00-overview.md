# AG Grid 学习研究索引

## 目标
- 逐步理解 AG Grid 项目的整体架构、设计思路与关键功能实现
- 为后续在其他仓库中独立实现一版 enterprise 能力建立知识地图
- 将学习过程中的结论、问题、线索和源码入口持续沉淀在本目录

## 建议学习主线
1. Monorepo 与包结构
2. ag-grid-community 核心职责
3. 渲染与虚拟化机制
4. Module / Feature 注册体系
5. Community 与 Enterprise 边界
6. 关键数据流与状态管理
7. 典型 enterprise 能力拆解
8. 框架封装层（React / Vue / Angular）
9. 测试体系与质量保障
10. 构建、文档与示例系统
11. AI / Agent 工作流设计

## 目录结构
### 根目录
- `00-overview.md`：总览、学习路线、阶段进度

### `meta/`
- `meta/01-learning-topics.md`：需要学习的主题清单
- `meta/10-learning-guide-agent.md`：学习引导 agent 的职责与协作方式
- `meta/20-learning-progress.md`：当前学习进度快照（仅保留当前状态）
- `meta/history/`：按专题归档的 checkpoint 历史记录
- `meta/99-questions-and-followups.md`：待确认问题、阅读线索、后续深入点

### `plans/`
- `plans/02-learning-roadmap-v1.md`：阶段化学习路线
- 后续专题教案也统一放在 `plans/` 下

### `notes/`
- `notes/09-ai-agent-workflow.md`：AI / Agent 工作流设计与可推断机制
- 后续正式学习笔记统一放在 `notes/` 下

### `../.claude/skills/`
- `.claude/skills/ag-grid-guided-learning/SKILL.md`：开课入口 skill，负责启动“关键 checkpoint 提问 + checkpoint 进度更新”的学习模式

## 管理约定
- `plans/` 只放教案、学习路线、专题教学拆解，不放最终学习结论
- `notes/` 只放阶段性确认后的正式学习笔记
- `meta/` 放索引、方法说明、待确认问题、当前进度快照与专题历史等元信息
- 教学过程采用“前台主 assistant 连续讲解 + 后台 agent 备课 + 关键 checkpoint 提问”的模式
- 到达 checkpoint 时同时更新 `meta/20-learning-progress.md` 与对应 `meta/history/<topic>.md`，记录当前专题、已确认理解和下一步

## 当前状态
- 已确认学习记录统一归档到 `research-notes/`
- 已确认采用更规整的三层结构：`meta/`、`plans/`、`notes/`
- 已确认学习路线采用“先建立架构地图，再优先关注对自研 enterprise 最有价值部分”的混合策略
- 已确认学习模式：由当前主 agent 负责规划路线与阶段大纲，再由后台 teaching aide agent 提供 teaching brief，由主 assistant 以前台“连续讲解 + 关键 checkpoint 提问”的方式带学
- 已确认教案和正式学习笔记分开管理
- 已确认学习进度采用独立进度文件，并且只在 checkpoint 完成时更新
- 当前具体推进位置以 `meta/20-learning-progress.md` 为准，已完成 checkpoint 历史见 `meta/history/`
