# AI / Agent 工作流研究

## 为什么这部分值得研究
- 它不是单纯的“AI 配置文件”，而更像一套围绕 monorepo 开发流程设计的工程化工作流
- 如果未来你要在自己的项目里引入 agent，这部分比单看 prompt 更有迁移价值
- 它还可能反向帮助理解团队是怎么组织知识、约束开发行为、分发任务与沉淀经验的

## 当前可见证据
### 1. `.gitignore` 暗示了“共享规则”与“本地状态”的分层
- 仓库忽略了大量 AI 工具目录与本地配置，如 `.claude/`、`.cursor/`、`.gemini/` 等
- 但又特意保留了 `.rulesync/.aiignore`
- 这说明团队在区分：
  - 应纳入版本管理的共享 agent 配置
  - 不应纳入版本管理的个人/本地工具状态

参考：
- [`.gitignore:78-145`](../.gitignore#L78-L145)

### 2. `.rulesync/` 是这套工作流的核心入口
- `.rulesync/README.md` 明确把 `.rulesync/` 定义为 canonical shared source
- 并指出 `.claude/` 更像 Claude Code 的工具侧扩展或镜像

参考：
- [`.rulesync/README.md:5-10`](../.rulesync/README.md#L5-L10)

### 3. 能力模型已经很清晰
从 README 可以直接读出 4 类机制：
- **rules**：基于文件模式自动加载
- **skills**：按需调用
- **sub-agents**：当任务匹配时自动分派
- **commands**：显式触发

参考：
- [`.rulesync/README.md:12-17`](../.rulesync/README.md#L12-L17)

### 4. 这不是玩具配置，而是贴合实际开发流程
仓库里已经按真实工作拆好了很多能力：
- everyday development
- testing and quality
- documentation and examples
- planning and analysis
- memory
- git and branch management

这说明它服务的不是“和 AI 聊天”，而是**把仓库知识和团队流程结构化给 agent 使用**。

参考：
- [`.rulesync/README.md:26-97`](../.rulesync/README.md#L26-L97)

## 目前可以较可靠倒推的工作流模型
### 一层：共享知识源
- 以 `.rulesync/` 作为跨工具共享定义
- 里面沉淀规则、技能、命令、子代理描述、MCP 配置入口

### 二层：工具适配层
- 针对 Claude / Cursor / Gemini 等工具做各自接入
- 仓库选择把这些本地工具目录忽略掉，避免把个人环境状态提交进来

### 三层：任务分发层
- 简单的上下文约束靠 rule 自动加载
- 显式任务通过 skill / command 触发
- 更专门的工作通过 subagent 自动承接

### 四层：知识沉淀层
- `remember` / `recall` 一类能力说明他们不仅在做“单次辅助”，也在做“上下文延续”
- `plan-review` / `plan-implementation-review` 说明规划与执行也是工作流的一部分

## 当前还不能确定的部分
以下内容目前只能弱推断，不能当作事实：
- 团队成员日常最常用哪些 skill
- subagent 的真实触发条件是否还有额外 heuristics
- `.rulesync` 与工具目录之间是否存在自动同步脚本及其细节
- external/ag-shared 在这套工作流里的真实权重和演化方式
- 团队是否强制要求某些开发阶段必须走 agent 工作流

## 推荐后续研究问题
1. `.rulesync/skills/` 中每个 skill 的职责边界是什么
2. `.rulesync/rules/` 的触发模式如何映射到仓库目录结构
3. `subagents/` 里的角色分工怎样支持 monorepo 开发
4. `setup-prompts` 与相关脚本如何把这套配置落到本地环境
5. 哪些设计适合未来迁移到你自己的 enterprise 项目里

## 初步结论
- 这套 agent 工作流**可以研究，而且值得纳入主学习线**
- 我们能比较可靠地还原它的**结构与设计意图**
- 但对于团队真实日常使用习惯，需要把“证据”和“推断”分开记录
