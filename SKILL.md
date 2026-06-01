---
name: git-conventions
description: Git 规范助手 — 初始化项目、模块提交推送、生成仓库文档（README/CHANGELOG）、Issue 管理。触发词：初始化项目、连接git、提交推送、生成文档、写commit message、提交issue。
---

# Git 规范助手

简化版 Git 工作流 + 文档生成 + Issue 管理。

---

## 前置条件

### GitHub CLI 检测

使用 Issue/PR 功能前，需确认 gh CLI 状态：

```bash
# 检测安装
where gh

# 检测登录状态
gh auth status

# 未登录时提示
gh auth login
```

---

## 一、初始化项目

**触发词**：「初始化项目」「连接git」

### 工作流

```
1. 确认项目目的 → 用户说明
2. 初始化 Git → git init
3. 创建 .gitignore → 按项目类型
4. 连接远程 → git remote add origin <url>
5. 首次提交 → git add . && git commit -m "feat: initial commit"
6. 推送 → git push -u origin main
```

### .gitignore 模板

| 项目类型 | 忽略内容 |
|:--------:|:---------|
| Node | `node_modules/`, `dist/`, `.env` |
| Python | `__pycache__/`, `.venv/`, `*.pyc` |
|通用| `.DS_Store`, `Thumbs.db`, `*.log` |

---

## 二、提交推送

**触发词**：「提交」「推送」「写commit message」

### Commit 格式

```
<type>(<module>): <desc>
```

| type | 用途 | 示例 |
|:----:|:-----|:-----|
| `feat` | 新功能 | `feat(core): add plan generator` |
| `fix` | 修复 | `fix(ui): resolve layout issue` |
| `docs` | 文档 | `docs: update README` |
| `refactor` | 重构 | `refactor(utils): simplify logic` |
| `chore` | 杂项 | `chore: update dependencies` |

### 工作流

```
1. 查看变更 → git status && git diff --staged
2. 分析模块 → 确认 <module> 范围
3. 生成消息 → 按格式生成，≤ 50 字
4. 等待确认 → 用户同意后执行
5. 提交 → git commit -m "<message>"
6. 推送 → git push (用户指令)
```

### 模块命名规则

- 按目录/功能划分：`core`, `ui`, `api`, `docs`, `utils`
- 单文件变更：用文件名（去掉扩展名）

---

## 三、生成仓库文档

**触发词**：「生成文档」「初始化仓库文档」「生成 README」

### 输出要求

- **双语**：中文版主文件，英文版 `README.en.md`
- **Icon**：使用 Shields.io徽章显示环境
- **风格**：干净优雅，表格对齐，无冗余

### README 结构

```markdown
# 项目名称

<p align="center">
  <img src="https://img.shields.io/badge/Node-18.x-339933?logo=node.js" />
  <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript" />
  <img src="https://img.shields.io/badge/License-MIT-blue" />
</p>

> 一句话描述项目用途

---

## 功能

| 功能 | 说明 |
|:-----|:-----|
| 功能A | 描述 |
| 功能B | 描述 |

---

## 快速开始

\`\`\`bash
# 安装
npm install

# 运行
npm start
\`\`\`

---

## 目录结构

\`\`\`
project/
├── src/
│   ├── core/       # 核心模块
│   └── utils/      # 工具函数
└── docs/           # 文档
\`\`\`

---

## 许可证

MIT
```

### 徽章模板

| 环境 |徽章 |
|:-----|:-----|
| Node | `https://img.shields.io/badge/Node-18.x-339933?logo=node.js` |
| Python | `https://img.shields.io/badge/Python-3.10-3776AB?logo=python` |
| TypeScript | `https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript` |
| React | `https://img.shields.io/badge/React-18.x-61DAFB?logo=react` |
| Vue | `https://img.shields.io/badge/Vue-3.x-4FC08D?logo=vue.js` |

### 工作流

```
1. 扫描项目 → package.json / 目录结构
2. 确认环境 → Node/Python/框架版本
3. 生成中文版 → README.md
4. 生成英文版 → README.en.md
5. 确认内容 → 用户审核
```

---

## 四、CHANGELOG 生成

**触发词**：「生成 CHANGELOG」

### 格式

```markdown
# CHANGELOG

## [1.0.0] - 2024-01-01

### Added
- 新功能描述

### Fixed
- 修复描述

### Changed
- 变更描述
```

### 工作流

```
1. 获取版本 → git tag --sort=-v:refname
2. 获取提交 → git log <tag1>..<tag2> --oneline
3. 分类整理 → Added / Fixed / Changed
4. 生成文件 → CHANGELOG.md
```

---

## 五、Issue 管理

**触发词**：「提交 issue」「创建 issue」「新建 issue」

### 前置检测

```
1. 检测 gh CLI → where gh
2. 检测登录状态 → gh auth status
3. 未登录提示 → 请先运行 gh auth login
```

### Issue 模板

| 类型 | 标题前缀 | 内容模板 |
|:-----|:---------|:---------|
| bug | `fix:` | 问题描述、复现步骤、期望结果 |
| feature | `feat:` | 功能描述、使用场景、验收标准 |
| enhancement | `enhance:` | 增强描述、改进方案 |

### 工作流

```
1. 确认仓库 → git remote -v 或用户指定
2. 输入标题 → 用户输入或从参数解析
3. 输入内容 → 支持 markdown
4. 选择标签 → bug/feature/enhancement（可选）
5. 确认创建 → 用户同意后执行
6. 执行创建 → gh issue create --repo <repo> --title "<title>" --body "<body>"
7. 返回链接 → 输出 Issue URL
```

### 使用示例

```
用户: 提交 issue 到 skills-git-conventions，标题是 "fix: 启动崩溃"
AI: 正在创建 Issue...
    仓库: ice-deep-dream/skills-git-conventions
    标题: fix: 启动崩溃
    内容: (请输入详细描述)
    确认创建？(y/n)
用户: y
AI: ✅ Issue 已创建: https://github.com/ice-deep-dream/skills-git-conventions/issues/1
```

---

## 六、Pull Request

**触发词**：「创建 PR」「提交 PR」「新建 PR」

### 工作流

```
1. 检测当前分支 → git branch --show-current
2. 推送分支 → git push -u origin <branch>（如未推送）
3. 输入标题 → 用户输入或从 commits 提取
4. 输入描述 → 支持 markdown 模板
5. 确认创建 → gh pr create
6. 返回链接 → 输出 PR URL
```

---

## 注意事项

1. **不自动执行** — 所有 git 操作等待用户确认
2. **先查状态** — 提交前先 `git status`
3. **简洁描述** — commit message ≤ 50字
4. **模块划分** — 按 功能/目录 确定scope
5. **gh CLI 检测** — Issue/PR 功能需确认 gh 已登录