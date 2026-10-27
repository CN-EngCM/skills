---
task: YYYY-MM-DD-short-name
kind: dev            # dev | test
status: in-progress  # pending | in-progress | review | done | paused | abandoned
lock: ""             # "<role> · <agent> · YYYY-MM-DD HH:mm"，空 = 未锁
next_role: planner   # planner | implementer | tester | user
round: 0             # 当前轮次；首次实施或测试前为 0
---

# <任务标题>

<一两句话说明任务目标>

## 下一步

<角色>：<要做什么>。输入：<要读的文档>。产出：<要写的文档>。

## 文档

- [requirements.md](requirements.md) — 草稿 / 定稿 / 跳过（原因：……）
- [design.md](design.md) — ……
- [test-plan.md](test-plan.md) — ……
- [impl-log-r1.md](impl-log-r1.md) — ……
- [test-report-r1.md](test-report-r1.md) — 通过 / 未通过（fail N，blocked N）

## 历史

| 时间 | 角色 | agent | 动作 | 产出 |
|---|---|---|---|---|
| YYYY-MM-DD HH:mm | planner | claude | 新建任务 | README.md |
