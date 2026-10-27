---
type: impl-log
round: N
role: implementer
agent: <agent>
date: YYYY-MM-DD
inputs: [design.md]   # r2 起为 [test-report-r(N-1).md]
---

# 实施记录 rN：<标题>

## 输入

<r1：design.md；r2 起：test-report-r(N-1).md>

## 完成情况

<!-- r1 按实施步骤记录；r2 起按用例记录 -->

- S1 ✅ <说明>
- S2 ⚠️ 部分完成（原因：……）
- TC-03 ✅ <失败原因与修法>

## 改动文件

- `<路径>` — <改动内容>

## 与方案的偏差

<没有就写"无"，不能省略本节>

## 自检

<构建、单元测试的命令和结果>

## 建议回归的用例

<受本轮改动影响的用例 ID；没有就写"无">

## 测试代码问题

<认为测试脚本写错的地方，由 tester 下轮修正；没有就写"无">

## 需交规划者决定

<有内容时下一步交给 planner 而不是 tester；没有就写"无">
