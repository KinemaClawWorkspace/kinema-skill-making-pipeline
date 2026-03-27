---
name: kinema-skill-making-pipeline
description: |
  KinemaClaw Skill 开发与发布规范。定义 skill 开发、版本管理、发布的标准化流程。
  所有在 KinemaClaw 构建的 skill 必须遵循此规范。
  触发条件：创建新 skill、发布 skill、修改现有 skill。
---

# Kinema Skill 开发与发布规范

本 skill 定义了 KinemaClaw ecosystem 中 skill 的开发、版本管理、发布的标准化流程。

## 核心原则

1. **Git 优先** - 所有修改必须在 Git 仓库中管理
2. **原子化提交** - 每次 commit 必须是有意义的独立变更
3. **版本化发布** - 发布前必须打 Git tag
4. **禁止原位发布** - 不允许发布 /app/skills/ 中的原始 skill
5. **必须有 onboarding** - 每个 skill 必须有安装/配置引导

## 开发流程

### 1. 创建 Skill 仓库

```
projects/<skill-name>/
├── SKILL.md              # 必须：skill 定义
├── scripts/              # 可选：自动化脚本
├── references/           # 可选：参考资料
└── 其他项目文件
```

### 2. 开发规范

- **原子化 commit**: 每个 commit 只做一件事
- **及时提交**: 完成一个功能/修复一个问题后立即 commit
- **清晰 message**: 使用描述性的 commit message

```bash
# 好
git commit -m "Add install command to searxng-search CLI"

# 不好
git commit -m "update" 
git commit -m "fix stuff"
```

### 3. 发布流程

```bash
cd projects/<skill-name>

# 1. 确保所有修改已 commit
git status

# 2. 打版本 tag (语义化版本)
git tag -a v1.2.0 -m "Release v1.2.0: Add onboarding"

# 3. 推送 tag 到 GitHub
git push origin v1.2.0

# 4. 在 GitHub 仓库发布
# Settings → Releases → Create new release

# 5. 推送到 ClawHub
clawhub publish . --slug <skill-name> --version 1.2.0
```

### 版本号规则

遵循语义化版本 (Semantic Versioning):
- **MAJOR**: 不兼容的 API 变更
- **MINOR**: 向后兼容的新功能
- **PATCH**: 向后兼容的 bug 修复

```
v1.0.0 → 首次发布
v1.1.0 → 新功能
v1.1.1 → bug 修复
v2.0.0 → 重大变更
```

## 必须要素

每个 skill 必须包含：

### 1. SKILL.md

包含：
- name: skill 名称
- description: 功能描述和触发条件
- 完整使用说明

### 2. Onboarding

必须有以下之一：
- **文本 onboarding**: 在 SKILL.md 中包含安装、配置、启动说明
- **脚本 onboarding**: 提供自动化安装/配置脚本

推荐包含：
- 一键安装命令
- 配置说明（环境变量等）
- 启动/停止方法
- 故障排除

### 3. 禁止内容

skill 中**禁止**包含：
- 个人网站、域名
- 密码、账号、Token
- 个人邮箱、电话
- 真实姓名、身份信息

## GitHub 仓库规范

- 默认创建 **Private** 仓库
- 使用有意义的仓库名称
- 保持 README.md 与 SKILL.md 一致

## 目录结构

```
projects/
├── alist-cli/           # Git 仓库
│   ├── SKILL.md
│   ├── scripts/
│   └── README.md
├── searxng-search/      # Git 仓库
│   ├── SKILL.md
│   └── scripts/
└── concept-research/   # Git 仓库
    └── SKILL.md
```

## 自动化脚本示例

```bash
#!/bin/bash
# skill-publish.sh - 发布 skill 到 GitHub + ClawHub

SKILL_NAME=$1
VERSION=$2

if [ -z "$SKILL_NAME" ] || [ -z "$VERSION" ]; then
    echo "Usage: $0 <skill-name> <version>"
    exit 1
fi

cd projects/$SKILL_NAME

# 检查是否有未提交的修改
if ! git diff --quiet; then
    echo "Error: 先提交所有修改"
    exit 1
fi

# 打 tag
git tag -a v$VERSION -m "Release v$VERSION"

# 推送
git push origin master
git push origin v$VERSION

# 发布到 ClawHub
clawhub publish . --slug $SKILL_NAME --version $VERSION
```

## 相关文档

- [ClawHub 文档](https://docs.openclaw.ai)
- [Skill 创建规范](/app/skills/skill-creator/SKILL.md)
