# 第二讲历史：ag-grid-community 核心职责

## 主题
- `ag-grid-community 核心职责`

## Checkpoint 历史
### 回填记录 1
- 已完成 checkpoint：第二讲第一个 checkpoint：`ag-grid-community` 的三层底座职责理解（基础能力 / 公共契约 / 展示主题）
- 新增理解：`ag-grid-community` 不只是社区版功能集合，更像平台底座，同时承担三层底座职责；当前已能用“三个短词”概括为：基础能力、通信契约、样式支持
- 当时下一步：进入 `ag-grid-community` 的基础能力层 / 运行时骨架

### Checkpoint 2（2026-03-27）
- 已完成 checkpoint：`ag-grid-community` 基础能力层 / 运行时骨架的第一轮理解
- 新增理解：已能把启动链路压缩为 `createGrid(...) -> GridCoreCreator -> AgContext / beans -> createUi -> CtrlsService ready -> SyncService.start() -> ColumnModel / rowModel 启动 -> gridReady`
- 关键分工校准：`GridCoreCreator` 不只是加载用户配置，而是负责组装 grid runtime；`CtrlsService` 不只是维护 controller 状态，而是作为关键 UI controllers 的注册与 ready 协调中心；`SyncService` 不只是“执行组���启动”，而是在 controllers ready 后推进首轮列/行模型启动并发出 `gridReady`
- 当时下一步：继续拆 `ColumnModel`、`rowModel` 与首轮启动顺序，建立更细的核心对象协作图

### Checkpoint 3（2026-03-27）
- 已完成 checkpoint：`ColumnModel` 作为结构中枢的第一轮理解
- 新增理解：已能区分 `ColumnModel` 与 `SyncService` 的职责边界；`SyncService` 负责协调启动时机，但不持有核心状态；`ColumnModel` 则负责把 `columnDefs` 编译成 runtime 列结构，并作为多个列相关服务的共同上游
- 关键校准：`ColumnModel` 不是“存列配置”的被动对象，而是列树/列集合的生成器、扩展列装配器、相关服务的对齐入口以及列变化事件源头之一
- 当时下一步：进入 `rowModel.start()` 之后的启动顺序，继续理解为什么 `gridReady` 会在当前阶段发出

### Checkpoint 4（2026-03-27）
- 已完成 checkpoint：`gridReady` 发出语义的第一轮理解
- 新增理解：`gridReady` 的语义更接近 runtime ready / model ready / API ready，而不是“所有视觉内容已经完全渲染结束”；它的触发建立在关键 controllers ready、列模型建立、行模型启动之后
- 关键校准：`gridReady` 不是“表格整体初始化全部完成”的信号，而是“外部现在可以把这套 grid runtime 当成可用对象来交互”的信号
- 当时下一步：继续进入 `rowModel.start()` 之后的更细链路，理解首屏渲染与 `gridReady` 的边界
