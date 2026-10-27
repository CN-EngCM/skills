# skills

个人的 Agent Skills 集合,遵循 `SKILL.md` 规范(每个技能一个目录,目录内含带 YAML frontmatter 的 `SKILL.md`)。

## 技能列表

| 技能 | 说明 |
|---|---|
| [agent-flow](agent-flow/SKILL.md) | 多 agent 协作的文档与交接流程:需求 → 方案 → 测试清单 → 实施 → 测试 → 修复循环。仅通过 `/agent-flow` 显式调用。 |

## 安装

把技能目录复制到 Claude Code 的技能目录即可:

```bash
# 全局可用
cp -r agent-flow ~/.claude/skills/

# 仅当前项目可用
cp -r agent-flow <项目根目录>/.claude/skills/
```

## License

[MIT](LICENSE)
