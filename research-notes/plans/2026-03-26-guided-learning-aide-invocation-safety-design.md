# Guided Learning Aide Invocation Safety Tightening Design

## 背景
最近在排查 AG Grid guided-learning 会话异常时，暴露出的不是教学节奏或 checkpoint 机制本身的问题，而是 **`ag-grid-teaching-aide` 的调用与失败处理方式不够收紧**。

从会话 `9b1507ed-9091-4b55-9f49-6d96254aa728` 的最后几条记录可以看到：
- 主 assistant 在进入新 checkpoint 时默认调用了 `ag-grid-teaching-aide`
- 该调用使用了 `isolation: "worktree"`
- aide 启动后很早就在 `Read` 调用上失败，错误为 `Invalid pages parameter: ""`

这说明当前需要补的不是学习结构设计，而是 **aide 调用纪律、工具参数纪律、失败回退纪律**。

## 目标
只对 guided-learning 的最小调用流程做收紧，保证：
- `ag-grid-teaching-aide` 默认以轻量只读方式工作
- aide 失败不会把当前教学回合卡住
- 技能规则里明确禁止空参数字段这类脏调用

## 非目标
- 不改 AG Grid 学习路线或专题顺序
- 不改 checkpoint-level aide trigger 策略
- 不改 progress snapshot / history / notes 存储设计
- 不把这套规则推广到本仓库所有 agent，只限制在 guided-learning / teaching-aide 流程

## 设计概览
本次只修改两个文件：
- `.claude/skills/ag-grid-guided-learning/SKILL.md`
- `.claude/agents/ag-grid-teaching-aide.md`

收紧三条硬规则：
1. **默认不使用 worktree**
2. **aide 失败时主 assistant 必须显式告知并 fallback 到本地探索**
3. **示例和规则中禁止空参数字段**

## 具体规则

### 1. `ag-grid-guided-learning` 的调用规则
在 skill 中明确：
- 调用 `ag-grid-teaching-aide` 时，默认按当前工作区原地只读探索理解，不使用 worktree
- 只有明确需要隔离写文件时，才允许例外；普通备课、证据收集、teaching brief 准备都不得默认使用 worktree
- 如果 aide 在启动、读文件、搜索证据阶段失败，主 assistant 必须：
  1. 明确告知用户 aide 调用失败
  2. 立即改用本地 `Read` / `Grep` 继续完成这一轮备课
  3. 不因为 aide 失败而终止当前教学回合

### 2. `ag-grid-teaching-aide` 的运行规则
在 agent 中明确：
- 该 agent 的默认工作方式是轻量只读探索，不依赖 worktree
- 读取普通文本文件时，不应传 `pages`
- 不应传空字符串或空值参数字段给工具
- 如果前几步工具调用失败，不要继续堆叠更多失败操作；应停止进一步工具尝试，并返回一段简短失败摘要，让主 assistant 能接手本地探索

### 3. 示例与文案纪律
在 skill / agent 文案中避免给出会诱导错误调用的示例：
- 不出现空 `pages`、空字符串字段等参数模式
- 不暗示只读备课型 aide 应默认放进 worktree
- 不把 aide 失败描述成必须中止教学的条件

## 预期效果
补丁完成后，guided-learning 流程会更稳：
- checkpoint-level aide 仍然默认参与
- 但 aide 调用成本更低、失败面更小
- 即便 aide 失败，也不会让主教学流程卡死
- 工具调用示例更干净，减少参数层面的低级错误

## 验证要点
实现后应检查：
- `SKILL.md` 是否明确把“默认不用 worktree”写成硬规则
- `SKILL.md` 是否明确要求 aide 失败时显式告知用户并 fallback 到本地 `Read/Grep`
- `ag-grid-teaching-aide.md` 是否明确禁止空参数字段与普通文本读取时传 `pages`
- 两个文件是否都没有把该补丁扩展成更大范围的 agent 通用规则
