---
name: kinema-skill-making-pipeline
description: |
  KinemaClaw 跨平台 Skill 开发与发布规范。用于创建新 skill、修改或审查既有 skill、生成 Codex/Claude plugin manifest、同步版本、登记 marketplace，以及执行 GitHub Release 和 ClawHub 发布流程。
---

# Kinema Skill Making Pipeline for Codex

这是 Codex 发现入口。完整规范只在仓库根目录维护一份。

## 执行前读取

在创建、修改、审查或发布 skill 前，完整读取：

1. [完整开发与发布规范](../../SKILL.md)
2. [环境配置](../../references/ONBOARDING.md)

执行发版时还必须完整读取 [发版流程](../../references/release-process.md)；首次把新 skill 加入 marketplace 时还必须完整读取 [marketplace 发布流程](../../references/marketplace-publishing.md)。

## Codex 规则

- 将根规范中的 Agent 理解为当前 Codex agent。
- 新建或适配跨平台插件时，必须同时维护 `.claude-plugin/plugin.json`、`.codex-plugin/plugin.json` 和 `skills/<skill-name>/SKILL.md`。
- Codex skill 入口的 YAML frontmatter 只使用 `name` 和 `description`；版本以根 `SKILL.md` 和两个 plugin manifest 为准。
- 不复制完整规范到 Codex skill 入口；使用相对链接引用根文件和 `references/`，保持单一事实来源。
- 使用 `codex plugin marketplace ...` 和 `codex plugin add ...` 管理 Codex marketplace 与插件，不要求 Node.js。
- 插件安装或更新成功后，提醒用户新开 Codex 对话以加载新的 skill 内容。
