# AG Grid 第一版学习路线

## 学习策略
采用“混合型”路线：
- 前半段先建立 AG Grid 的整体架构地图，避免一开始陷入局部实现细节
- 后半段尽快转向对“自研 enterprise”最有迁移价值的核心抽象
- 学习过程中同步保留一条 AI / Agent 工作流研究线，作为工程方法论补充

## 学习分工
### 主 assistant（前台）负责
- 规划阶段路线
- 明确每阶段目标
- 整理学习大纲与问题清单
- 决定当前教学块切入点
- 以前台“一讲一问”的方式带学
- 在专题结束后统一整理正式学习笔记

### 学习引导 agent（后台）负责
- 按阶段大纲深入搜索源码
- 围绕当前教学块产出 teaching brief
- 提供关键目录、关键类、关键链路与证据入口
- 提炼 facts / inferences / reusable ideas
- 协助补充教案与专题 notes 的增量内容

---

## Phase 1：建立全局地图
### 目标
先搞清楚 AG Grid 这个 monorepo 的整体结构、核心包边界，以及哪些模块是后续深入的主入口。

### 为什么先学这个
如果一开始直接钻 enterprise 功能，会很容易只看到“功能点”，看不到“承载这些功能的底层结构”。

### 包含主题
1. Monorepo 与包结构
2. ag-grid-community 核心职责
3. 构建、文档与测试体系的整体位置
4. AI / Agent 工作流的仓库级入口

### 推荐输出物
- 画出仓库结构图
- 列出后续高频阅读目录
- 标出 community / enterprise / wrappers / testing / docs 的边界
- 列出 agent 工作流的核心入口目录与概念

### 预期正式笔记
- `notes/01-monorepo-and-packages.md`
- `notes/08-build-and-docs.md`
- `notes/09-ai-agent-workflow.md`（先做结构层梳理）

---

## Phase 2：建立内核认知
### 目标
理解 grid 内核是如何运转的：配置如何进入系统、内部对象如何协作、渲染如何组织、性能优化依赖什么机制。

### ���什么这是关键
未来你自己写 enterprise，必须先知道“什么属于不可轻易破坏的 core 能力”。

### 包含主题
1. 核心数据流与状态管理
2. 渲染管线与虚拟化
3. 组件扩展机制
4. Row Model 基础认知

### 推荐输出物
- Grid 初始化到首屏渲染的主链路图
- service / controller / model / bean 的协作图
- 行列虚拟化的关键机制总结
- 自定义 renderer / editor / filter 的接入模型总结

### 预期正式笔记
- `notes/02-rendering-and-virtualisation.md`
- `notes/04-core-data-flow.md`
- 后续可补 `notes/10-component-extension-model.md`

---

## Phase 3：理解可扩展性设计
### 目标
把 AG Grid 从“一个表格组件”看成“一个可装配平台”，重点研究模块系统、扩展点与 community / enterprise 边界。

### 为什么这是自研 enterprise 的核心
你未来不是复制一个 grid，而是要设计一套**能承载高级能力的基础平台**。

### 包含主题
1. Module / Feature 注册机制
2. Community 与 Enterprise 的边界设计
3. Row Model 体系
4. 组件扩展与功能装配的统一模式

### 推荐输出物
- module 注册模型图
- community / enterprise 分层图
- “哪些能力必须在 core 预留扩展点”清单
- “可迁移设计模式”清单

### 预期正式笔记
- `notes/03-module-system.md`
- `notes/05-enterprise-features-map.md`（先写总图）
- 可补 `notes/11-row-models.md`

---

## Phase 4：拆 enterprise 能力地图
### 目标
不再只看抽象，而是把 enterprise 中典型功能拆开，看它们分别依赖了哪些 core 能力。

### 包含主题
1. Row Grouping / Aggregation / Pivoting
2. Server-Side Row Model
3. Range Selection / Integrated Charts
4. Master / Detail / Excel Export / Advanced Filter

### 推荐输出物
- enterprise 功能依赖矩阵
- 每个功能的源码入口与关键链路
- 哪些功能适合自研第一批实现，哪些可以延后

### 预期正式笔记
- `notes/05-enterprise-features-map.md`
- 可继续拆成多个子专题文件

---

## Phase 5：理解对外适配与工程支撑
### 目标
理解 AG Grid 怎么把 core 适配到 React / Vue / Angular，并通过测试、文档、示例和工程化体系保证可维护性。

### 包含主题
1. 框架封装层
2. 测试体系
3. 文档与示例系统
4. 构建与发布支撑

### 推荐输出物
- wrapper 与 core 边界图
- 测试金字塔 / 测试分层图
- docs / examples / tests 的联动关系总结

### 预期正式笔记
- `notes/06-framework-wrappers.md`
- `notes/07-testing-and-verification.md`
- `notes/08-build-and-docs.md`

---

## Phase 6：回到“自研 enterprise”抽象
### 目标
把前面学到的内容重新压缩成你自己的实现框架，而不是停留在“我看懂了 AG Grid”。

### 关键问题
- 你的最小可行 core 应该包含哪些能力
- 哪些 enterprise 能力可以作为第一阶段目标
- 哪些扩展点必须提前设计
- 哪些 AG Grid 设计值得借鉴，哪些可以简化

### 推荐输出物
- enterprise 自研能力优先级清单
- 自研架构草图
- AG Grid → 自研设计映射表

---

## 第一阶段建议学习顺序（近期）
1. `notes/01-monorepo-and-packages.md`
2. `notes/09-ai-agent-workflow.md`（结构视角）
3. `notes/04-core-data-flow.md`
4. `notes/02-rendering-and-virtualisation.md`
5. `notes/03-module-system.md`
6. `notes/05-enterprise-features-map.md`

## 当前建议的学习节奏
- 先由主 assistant 在 `plans/` 中产出阶段大纲与专题教案
- 然后由 learning-guide 围绕当前教学块生成 teaching brief
- 主 assistant 再以前台“一讲一问”的方式推进学习
- 每学完一个专题，再把确认后的结论统一写入 `notes/`
- 每完成一个 phase，再回顾哪些结论对“自研 enterprise”真正有价值
