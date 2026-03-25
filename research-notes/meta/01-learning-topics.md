# AG Grid 需要学习的点

> 目的：先把“要学什么”列出来，后续再决定顺序、深度和阅读方式。

## 1. Monorepo 与仓库组织
- 仓库为什么采用 Nx + Yarn Workspaces
- `packages/`、`community-modules/`、`testing/`、`documentation/`、`plugins/`、`external/` 的职责分工
- `ag-grid-community`、`ag-grid-enterprise` 与各框架 wrapper 的依赖关系
- 核心包与文档、示例、测试之间如何联动

## 2. Community 核心包的职责边界
- `ag-grid-community` 负责哪些基础能力
- 什么能力是“内核级”的，什么能力是“功能模块级”的
- Community 暴露给外部的主要 API 面有哪些
- 配置项、事件、回调、组件接口是如何组织的

## 3. 渲染管线与虚拟化
- Grid 初始化后主流程如何启动
- Row / Cell / Header 的渲染链路是什么
- 行虚拟化、列虚拟化分别怎么工作
- DOM 复用策略、滚动时刷新策略、性能关键点有哪些
- 自定义渲染器是如何插入主渲染流程的

## 4. 核心数据流与状态管理
- `gridOptions` 如何进入系统并驱动运行时行为
- 内部 service / controller / model / bean 之间如何协作
- 事件系统如何分发
- 状态变化如何影响渲染与功能模块
- 生命周期关键阶段：创建、更新、销毁

## 5. Module / Feature 注册机制
- 模块化架构的核心抽象是什么
- 功能是如何注册到 Grid 中的
- community 与 enterprise 的功能装配差异是什么
- module 注册时会注入哪些能力（API、组件、service、feature）
- 如果自己做一版 enterprise，最值得借鉴的解耦点有哪些

## 6. Community 与 Enterprise 的边界设计
- enterprise 到底是在 community 之上“加模块”，还是“改内核”
- 哪些能力完全独立，哪些能力必须深度依赖核心能力
- license / feature gating 在架构层面如何体现
- 如何在不破坏 core 的情况下叠加高级能力

## 7. 关键 enterprise 功能实现地图
- Row Grouping
- Pivoting
- Aggregation
- Range Selection
- Integrated Charts
- Server-Side Row Model
- Master / Detail
- Excel Export
- Advanced Filter

针对每个功能都需要关注：
- 入口模块在哪
- 对 core 依赖哪些能力
- 新增了哪些数据结构 / 状态 / UI 层
- 是否有明显可抽象复用的模式

## 8. Row Model 体系
- Client-Side Row Model 的职责
- Infinite / Server-Side / Viewport 等 Row Model 的差异
- Row Model 如何影响排序、过滤、分组、分页、虚拟化
- 如果自己实现 enterprise，哪些 Row Model 能力必须先打底

## 9. 框架封装层
- React / Vue / Angular wrapper 做了哪些事，哪些没做
- wrapper 与 core 的边界在哪
- props / events / slots(or templates) 如何映射到 core API
- 为什么 ag-grid 能做到 framework-agnostic core

## 10. 组件扩展机制
- Cell Renderer / Cell Editor / Filter / Floating Filter / Header Component 等扩展点的统一模型
- 用户自定义组件如何被实例化、挂载、销毁
- 框架组件与原生组件如何兼容
- enterprise 功能是否复用这套扩展机制

## 11. 样式与主题系统
- legacy themes 与新 theming API 的关系
- 样式层与渲染层如何配合
- 主题能力哪些是纯 CSS，哪些依赖 JS 逻辑
- 如果自己做 enterprise，主题系统需要复制到什么程度

## 12. 测试体系
- behavioural tests 为什么是主测试体系
- unit test、E2E、accessibility test 分别覆盖什么
- grid 行为类测试如何组织
- 示例、文档、测试之间如何互相校验
- 后续阅读源码时，哪些测试可以反向帮助理解实现

## 13. 构建与工程化体系
- Nx 在这个仓库里承担了什么职责
- package build、type build、lint、test 的依赖关系
- examples、docs、plugins 是如何参与构建链路的
- 对外发布产物（community / enterprise / wrappers）是如何组织的

## 14. 文档与示例系统
- `documentation/ag-grid-docs` 的职责
- `_examples` 与正式文档页面如何关联
- 示例如何帮助定位具体功能源码入口
- 学习时如何利用 docs 和 examples 反向追踪实现

## 15. 设计思路层面的重点
- AG Grid 为什么坚持 framework-agnostic core
- 为什么采用模块化能力装配，而不是单体包设计
- 为什么 enterprise 能在同一套 core 上扩展
- 高性能表格的设计取舍：灵活性、性能、可扩展性之间怎么平衡
- 哪些架构思想值得迁移到你未来自己实现的 enterprise 项目中

## 16. AI / Agent 工作流设计
- 仓库为什么显式维护 `.rulesync/` 这类 agentic tooling 配置
- `.rulesync` 与 `.claude` / `.cursor` / `.gemini` 等工具目录的关系是什么
- rule / skill / command / subagent 各自的职责边界是什么
- 哪些能力会自动加载，哪些能力需要显式触发
- 这套 agent 工作流如何服务 monorepo 开发、测试、文档与代码评审
- 哪些部分可以从仓库静态信息中可靠推断，哪些只能作为弱推断
- 哪些设计值得迁移到你未来自己的项目中

## 17. 为“自己写一版 enterprise”提前抽象的问题
- 最小可行内核应该包含哪些能力
- enterprise 能力中哪些适合做第一批，哪些应该延后
- 哪些能力必须预留扩展点，否则后面很难补
- 哪些功能看似是“业务功能”，本质上其实是“底层架构能力”
- 哪些 AG Grid 设计值得借鉴，哪些不一定需要照搬

## 当前结论
- 学习不应直接按功能列表硬啃，而应先建立“架构地图”
- 后续学习路线需要同时兼顾：源码入口、抽象层次、实现复杂度、可迁移价值
