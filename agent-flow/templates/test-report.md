---
type: test-report
round: N
role: tester
agent: <agent>
date: YYYY-MM-DD
inputs: [test-plan.md, impl-log-rN.md]
---

# 测试报告 rN：<标题>

## 结论

通过 N / 失败 N / 阻塞 N / 跳过 N → **通过 / 未通过**

<!-- 通过 = 所有未废弃用例的最新结果都是 pass -->

## 本轮范围

<r1 写"全部"；r2 起写：自动化全部重跑 + 重测的手动用例 ID 及原因>

## 结果总览

<!-- 列出全部未废弃用例；本轮没测的在"本轮测试"列写"沿用 rK" -->

| 用例 | 结果 | 本轮测试 | 备注 |
|---|---|---|---|
| TC-01 | pass / fail / blocked / skip | 是 / 沿用 rK | |

## 失败与阻塞详情

### TC-XX <标题>

- 预期结果：
- 实际结果：
- 复现步骤：
  1. ……
- 证据：[tc-xx-xxx.png](attachments/rN/tc-xx-xxx.png)
- 初步判断（可选）：

## 测试清单问题

<发现清单本身有误时只记录，不修改；没有就写"无">

## 自动化测试代码

<本轮新增或修改的测试文件路径；没有就写"无">
