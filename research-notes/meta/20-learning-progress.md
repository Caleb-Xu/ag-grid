# 学习进度

## 当前专题
- 第二讲进行中：`ag-grid-community 核心职责`

## 当前 checkpoint
- 已完成：`ag-grid-community` 运行时骨架、`ColumnModel` 结构中枢角色与 `gridReady` 语义的第一轮理解：已能区分启动协调链路、列模型优先级，以及 `gridReady` 的 runtime ready 含义

## 当前有效理解
- `ag-grid-community` 当前已确认的最小心智模型：它是平台底座，同时承担基础能力、通信契约、样式支持三层职责
- 运行时骨架的关键分工已确认：`GridCoreCreator` 负责组装 grid runtime（合并配置、注册模块、准备 beans、创建 `AgContext`、创建 UI 并触发启动）；`CtrlsService` 负责收集并协调关键 UI controllers 的就绪状态；`SyncService` 负责在 controllers ready 后推进首轮列/行模型启动，并发出 `gridReady`
- `SyncService` 的补充定位已确认：它不是状态持有者，而是启动阶段的协调器；它统一处理“立即启动”和“`columnDefs` 延迟到达后再启动”两类场景
- `ColumnModel` 的补充定位已确认：它不是简单的列配置存储，而是列结构编译器与结构中枢；它把 `columnDefs` 转成 runtime 列树/列集合，并驱动 grouping、pivot、selection、visible columns 等后续服务对齐
- `gridReady` 的补充语义已确认：它更接近 runtime ready / model ready / API ready，而不是 full render finished；发出时机依赖的是关键 controllers ready 与列/行模型已启动，而不是所有视觉内容都完全稳定

## 下一步
- 继续第二讲：进入 `rowModel.start()` 之后的更细链路，看看行模型如何接住列结构上下文，以及首屏渲染和 `gridReady` 的边界怎么划

## 最近更新时间
- 2026-03-27
