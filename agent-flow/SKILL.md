---
name: agent-flow
description: 多 agent 协作的文档与交接流程（需求 → 方案 → 测试清单 → 实施 → 测试 → 修复循环），规范文档类型、命名、存放位置、角色权限和交接方式。仅在用户明确调用 /agent-flow 时使用，不要自动触发。
disable-model-invocation: true
argument-hint: "[new <描述> | <任务名> | archive <任务名>]"
---

# agent-flow

多个 agent 协作完成一个任务时，用本流程管理所有文档和交接。用户负责在 agent 之间中转；你每次只担任一个角色，完成一步，更新状态，交接，然后停下。

## 核心原则

1. **文档自包含**：接手的 agent 看不到之前的对话。你写的每份文档都必须让零上下文的 agent 能直接执行。禁止出现"按之前讨论的做"这类引用。
2. **待定即写出**：讨论过但没定下来的问题，写进"待定问题"小节，不得略过，也不得自行假设。
3. **不猜**：实施者遇到文档没说清的地方，停下，写进 impl-log 的"需交规划者决定"，交回规划者。
4. **守边界**：只写本角色允许写的东西（见"角色与权限"）。
5. **README 是唯一状态来源**：开工先读，完成必更新。

## 目录与命名

根目录：项目根目录（git 仓库根；无 git 则为当前工作目录）下的 `docs/agent-flow/`，首次 `new` 时创建。

```
docs/agent-flow/
├── 2026-10-27-user-login/     ← 进行中的任务
│   ├── README.md
│   ├── requirements.md
│   ├── design.md
│   ├── test-plan.md
│   ├── impl-log-r1.md
│   ├── test-report-r1.md
│   ├── impl-log-r2.md
│   ├── test-report-r2.md
│   └── attachments/
│       ├── r1/
│       └── r2/
└── archive/                   ← 已归档的任务，结构不变
```

- 任务文件夹：`YYYY-MM-DD-<短名>`，日期为创建日。
- 文件名、目录名只用英文小写、数字、连字符；正文用中文；frontmatter 字段值用英文枚举。
- 任务文件夹内**只允许**下表中的文件和 `attachments/rN/`，不得出现其他文件。
- 不维护全局索引。
- 文档之间、文档到附件一律用相对链接。

| 文件 | 内容 | 写入者 | 版本方式 |
|---|---|---|---|
| `README.md` | 任务状态 | 所有角色 | 原地更新 |
| `requirements.md` | 需求文档 | planner | 原地更新，文末追加变更记录 |
| `design.md` | 技术方案 | planner | 原地更新，文末追加变更记录 |
| `test-plan.md` | 测试清单 | planner | 原地更新，文末追加变更记录 |
| `impl-log-rN.md` | 实施记录 | implementer | 每轮新建；交给测试后不再修改 |
| `test-report-rN.md` | 测试报告 | tester | 每轮新建；写完不再修改 |
| `attachments/rN/` | 测试证据（截图、日志） | tester | 按轮次存放 |

**轮次**：`impl-log-rN` 与 `test-report-rN` 配对。r1 是首次实施及其测试，r2 是按 r1 报告做的修复及其回归，依此类推。测试任务（`kind: test`）没有 `impl-log-r1`，直接从 `test-report-r1` 开始。

创建任何文档前，先读本 skill 的 `templates/` 下对应模板，按模板结构写。

## 角色与权限

| 角色 | 职责 | 可写 | 不可写 |
|---|---|---|---|
| `planner` 规划者 | 与用户讨论需求，写需求、方案、测试清单 | requirements、design、test-plan | 产品代码、测试代码 |
| `implementer` 实施者 | 按方案写代码，按测试报告修代码 | 产品代码、impl-log | test-plan、test-report、测试代码 |
| `tester` 测试者 | 按清单执行测试，编写自动化测试 | test-report、attachments、测试代码 | 产品代码、test-plan |

- 所有角色都可以、也必须更新 README。
- 角色跟着步骤走，同一个 agent 在不同步骤可以担任不同角色。**你担任的角色就是 README 中 `next_role` 指定的角色**，用户无需另行说明。
- 测试代码放在项目原有的测试目录（按项目惯例），不放进 `docs/agent-flow/`。
- tester 发现测试清单本身有误：写进测试报告的"测试清单问题"，不改清单。
- implementer 认为测试代码有误：写进 impl-log 的"测试代码问题"，不改测试代码，由 tester 在下一轮修正。

## 生命周期

```
requirements → design → test-plan → impl-log-r1 → test-report-r1 → impl-log-r2 → test-report-r2 → … → review → done
```

两个入口（README 的 `kind`）：

- `dev` 开发任务：从 requirements 开始。
- `test` 测试任务：针对已有功能，从 test-plan 开始，没有 requirements 和 design。

任何阶段都可以跳过（例如小改动不写 requirements），在 README 的"文档"小节注明跳过及原因。

### 交接规则

完成一步后，按下表设置 README 的 `status` 和 `next_role`：

| 刚完成 | 条件 | status | next_role | 下一步 |
|---|---|---|---|---|
| requirements | — | pending | planner | 写 design |
| design | — | pending | planner | 写 test-plan |
| test-plan | dev 任务 | pending | implementer | 实施 r1 |
| test-plan | test 任务 | pending | tester | 测试 r1 |
| impl-log-rN | 有"需交规划者决定" | pending | planner | 解决问题、修改文档 |
| impl-log-rN | 否则 | pending | tester | 测试 rN |
| test-report-rN | 有"测试清单问题" | pending | planner | 先修正 test-plan，再按本表路由 |
| test-report-rN | 全部通过 | review | user | 等用户验收 |
| test-report-rN | 未通过，且连续未通过不足 3 轮 | pending | implementer | 修复，写 impl-log-r(N+1) |
| test-report-rN | 连续第 3 轮未通过 | paused | user | 停下，交用户决定 |
| planner 处理完上报 | — | pending | 视情况 | 回到被打断的角色 |

- 跳过某阶段时，直接按下一个未跳过的阶段路由。
- **全部通过** = 所有未废弃用例的最新结果都是 `pass`。`skip` 不算通过；不该测的用例应由 planner 标记为废弃。
- 实施中途上报规划者后，回来继续实施时，在同一份 `impl-log-rN` 末尾追加"续（日期）"小节，不新开轮次。
- `next_role` 与你当前的角色相同时（例如 planner 写完 requirements 接着写 design），征得用户同意后可在本会话继续；不同则必须停下交接。

## 调用方式

| 调用 | 行为 |
|---|---|
| `/agent-flow` | 只读：扫描 `docs/agent-flow/` 下（不含 `archive/`）各任务 README 的 frontmatter，列出任务、kind、status、next_role、round、lock，以及"下一步"的内容。不做其他事。 |
| `/agent-flow new <描述>` | 新建任务，你担任 planner（见下文）。 |
| `/agent-flow <任务名>` | 继续该任务的下一步（见下文）。 |
| `/agent-flow archive <任务名>` | 归档（见下文）。 |

**任务名匹配**：在 `docs/agent-flow/` 下（不含 `archive/`）按包含关系模糊匹配文件夹名。匹配到多个就列出来让用户选；一个都没有就告知用户，不要新建。

### new

1. 与用户确认：kind（dev / test）、英文短名、从哪个阶段开始。
2. 创建 `docs/agent-flow/YYYY-MM-DD-<短名>/README.md`（用模板），以 planner 身份上锁。
3. 开始与用户讨论，产出第一份文档。

### 继续任务

读 README，先检查：

- `status: done` 或 `abandoned`：告知用户，停止。
- `status: review`：请用户验收。用户确认通过 → `status: done`、`next_role: ""`，记入历史，建议归档；用户提出问题 → 按用户指示路由。
- `status: paused`：说明暂停原因，等用户决定。
- `lock` 非空：停下，展示锁信息（谁、什么角色、何时），不做任何修改。只有用户明确说"强制接手"才覆盖锁，并在历史中记录。
- 否则：担任 `next_role`，按"执行一步"进行。

用户要求暂停或放弃任务时，把 `status` 改为 `paused` 或 `abandoned`，记入历史。

### archive

- 只直接归档 `status` 为 `done` 或 `abandoned` 的任务；其他状态需用户再次确认。
- 先在 README 历史中追加归档记录，再把整个文件夹移到 `docs/agent-flow/archive/`，名称不变。

## 执行一步

1. **上锁**：README 设置 `status: in-progress`，`lock: "<role> · <agent> · YYYY-MM-DD HH:mm"`。agent 名用你所运行工具的小写名称（如 claude、codex、opencode）。时间从系统获取，不要猜。
2. **读输入**：README 的"下一步"以及本步骤需要的文档。
3. **干活**：按角色要求（见下文）完成工作，按模板写产出文档。
4. **交接**：更新 README——清空 `lock`，按交接规则设置 `status`、`next_role`、`round`；重写"下一步"（具体到接手者能直接执行：角色、做什么、读哪些文档、产出什么）；更新"文档"小节；在"历史"追加一行。
5. **收尾回复**：按下文格式结尾，然后停下。

### planner

- **requirements**：与用户讨论，定稿后再写。需求编号 R1、R2……
- **design**：写明涉及的文件或模块、编号的实施步骤 S1、S2……、验收标准 AC1、AC2……（每条对应到 R）。实施步骤用编号列表，不用复选框；完成情况由 impl-log 记录。
- **test-plan**：用例编号 TC-01、TC-02……，**永不复用**；不再需要的用例标题前加 `[废弃]`，不删除。必须有"测试环境"小节；项目的 AGENTS.md、CLAUDE.md 等已写明的启动方式，只引用，不重复。
- **处理上报**：修改相应文档，在其变更记录中写明原因，然后路由回被打断的角色。

### implementer

- **r1**：输入 design。按 S 步骤逐条实施，在 impl-log 中逐条记录完成情况。
- **r2 起**：输入 `test-report-r(N-1)`。逐个处理 fail / blocked 用例，记录原因和修法。
- "与方案的偏差"必须写，没有就写"无"。
- 交接前完成自检（构建、单元测试），把结果写进 impl-log。
- 列出"建议回归的用例"，供 tester 确定手动回归范围。
- 判断失败原因出在需求或方案本身（而非代码 bug）时，写进"需交规划者决定"。

### tester

- **r1**：执行全部未废弃用例。
- **r2 起**：自动化用例全部重跑；手动用例只测：上轮 fail / blocked 的用例、impl-log 的"建议回归"用例、你判断受影响的用例。
- "结果总览"列出全部未废弃用例的最新状态，本轮没测的标"沿用 rK"。
- 结果取值：`pass` / `fail` / `blocked`（因环境或前置条件无法执行）/ `skip`（有意不执行，备注写原因）。
- 证据放 `attachments/rN/`，文件名如 `tc-03-login-error.png`，报告中用相对路径引用。
- 本轮新增或修改的测试代码文件，列在报告的"自动化测试代码"小节。

## 收尾回复格式

每完成一步，用以下四行结尾：

```
✅ 完成：<做了什么，产出哪些文档，关键结果>
➡️ 下一步：<角色> — <要做什么>
📋 发给下一个 agent：/agent-flow <短名>
💾 建议提交：git commit -m "agent-flow(<短名>): <文档类型> [rN]"
```

- 不要自动执行 git commit，只给出建议。
- 多份文档用逗号分隔，如 `agent-flow(user-login): design, test-plan`；归档写 `agent-flow(user-login): archive`。
- 下一步是用户（验收、决定）时，📋 一行改为说明需要用户做什么。
