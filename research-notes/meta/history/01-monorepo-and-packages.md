# 第一讲历史：Monorepo 与包结构

## 主题
- `Monorepo 与包结构`

## Checkpoint 历史
### 回填记录 1
- 已完成 checkpoint：第一讲 `Monorepo 与包结构` 的核心 checkpoint（仓库一级分层、`packages/` 内部分层、community / enterprise / wrappers 职责边界、`ModuleRegistry.registerModules(...)`、`AllCommunityModule` 与运行时版本校验）
- 新增理解：AG Grid 仓库一级目录可先分为产品本体层（`packages/`）与外部支撑层（`testing/`、`documentation/`）；`packages/` 内部不是平铺的一堆包，而是以 `ag-grid-community` 为底座，上面分出 enterprise 能力层与 framework wrappers；wrappers 主要负责框架适配，不负责默认装配 enterprise 能力；`ModuleRegistry.registerModules(...)` 是把模块按契约装配进 grid 运行时的统一入口，同时会递归处理依赖并做版本/有效性校验；`AllCommunityModule` 是模块系统上的聚合入口，而不是反模块化的例外；版本同步脚本负责统一产出版本，`ModuleRegistry` 的运行时校验负责阻止用户把不兼容模块组合装到一起
- 当时下一步：现有 progress 文件未保留
