---
name: git-conventions
description: Git 规范助手 — 初始化项目、模块提交推送、生成仓库文档（README/CHANGELOG）、Issue 管理、部署 VitePress 文档站点。触发词：初始化项目、连接git、提交推送、生成文档、写commit message、提交issue、部署文档站点。
---

# Git 规范助手

简化版 Git 工作流 + 文档生成 + Issue 管理 + VitePress 文档站点部署。

---

## ⚠️ 核心原则（必须遵守）

1. **按步骤执行** — 每个阶段都有编号步骤，必须逐步执行并显示进度 `[1/N]`
2. **每个步骤都要询问** — 关键操作前必须询问用户确认
3. **自动检测下一步** — 完成一个阶段后，自动检测是否需要下一阶段
4. **输出状态报告** — 每个阶段完成后输出标准化报告

---

## 📋 执行清单（启动时必读）

当用户触发此 Skill 时，**必须按以下顺序检测并执行**：

```
□ 阶段一检测：Git 是否初始化？
  ├─ 未初始化 → 执行阶段一（项目初始化）
  └─ 已初始化 → 继续

□ 阶段二检测：是否为他人项目？
  ├─ 检测方法：查看 README 中的原作者信息
  ├─ 是他人项目 → 询问是否执行阶段二（项目私有化）
  └─ 自己的项目 → 跳过

□ 阶段三：等待用户指令（提交/推送等）
```

---

## 功能阶段总览

| 阶段 | 功能 | 触发词 | 执行时机 | 自动检测 |
|:----:|:-----|:-------|:---------|:--------:|
| **阶段一** | 项目初始化 | `初始化项目` `连接git` | 首次使用 skill / 新项目 | ✅ 自动检测 Git 状态 |
| **阶段二** | 项目私有化 | `私有化` `重写README` | 从他人项目改造时 | ✅ 自动检测原作者 |
| **阶段三** | 提交推送 | `提交` `推送` `写commit message` | 有代码变更时 | ❌ 用户触发 |
| **阶段四** | 生成仓库文档 | `生成文档` `生成README` | 项目成熟期 | ❌ 用户触发 |
| **阶段五** | CHANGELOG 生成 | `生成CHANGELOG` | 版本发布时 | ❌ 用户触发 |
| **阶段六** | Issue 管理 | `提交issue` `创建issue` | 发现问题/改进点时 | ❌ 用户触发 |
| **阶段七** | 部署文档站点 | `部署文档站点` `部署VitePress` | 需要文档网站时 | ❌ 用户触发 |
| **阶段八** | Pull Request | `创建PR` `提交PR` | 分支开发完成时 | ❌ 用户触发 |

---

## 阶段一：项目初始化

**触发词**：「初始化项目」「连接git」

**执行时机**：首次使用 skill / 新建项目

### 🔴 必须按步骤执行

```
[1/4] 检测 Git 状态
      ├─ git rev-parse --git-dir
      ├─ git remote -v
      └─ git branch --show-current

[2/4] 输出检测结果，询问用户：
      「检测到项目未初始化 Git，是否执行初始化？」

[3/4] 用户确认后执行：
      ├─ git init
      ├─ 检查/创建 .gitignore
      └─ 输出操作结果

[4/4] 输出状态报告，检测是否需要阶段二
```

### 场景判断逻辑

#### 场景 A：无 Git 初始化

```
检测：git rev-parse --git-dir 失败

执行步骤：
[1/4] 输出：「❌ 项目未初始化 Git」
[2/4] 询问：「是否执行 git init？」
[3/4] 用户确认 → git init → 检查/创建 .gitignore
[4/4] 输出状态报告
```

#### 场景 B：有 Git 无远程

```
检测：git remote -v 为空

执行步骤：
[1/4] 输出：「✅ Git 已初始化，但未连接远程」
[2/4] 询问：「是否连接远程仓库？」
[3/4] 用户输入地址 → git remote add origin <url>
[4/4] 输出状态报告
```

#### 场景 C：有 Git 有远程

```
检测：git remote -v 有内容

执行步骤：
[1/4] 输出：「✅ 项目已初始化并连接远程」
[2/4] 显示远程地址和当前分支
[3/4] 询问：「是否需要其他操作？」
[4/4] 检测是否需要阶段二
```

### 阶段一完成后自动检测

```
阶段一完成后，必须执行：

检测：README 中是否包含以下关键词？
  - "程序员鱼皮"
  - "编程导航"
  - "codefather"
  - 或其他明显不是 "Cryodream" 的作者信息

如果是他人项目 → 询问：「检测到这是他人项目，是否执行阶段二（项目私有化）？」
```

### .gitignore 模板

| 项目类型 | 忽略内容 |
|:--------:|:---------|
| Node | `node_modules/`, `dist/`, `.env` |
| Python | `__pycache__/`, `.venv/`, `*.pyc` |
| 通用 | `.DS_Store`, `Thumbs.db`, `*.log` |

### 状态报告模板

```
📊 项目初始化状态报告
─────────────────────────────────
Git 状态：    ✅ 已初始化 / ❌ 未初始化
远程仓库：    ✅ 已连接 / ❌ 未连接
当前分支：    main / master / <branch>
提交记录：    X 条提交

下一步操作：
□ 连接远程仓库（如未连接）
□ 创建首次提交（如无提交）
□ 推送到远程

🔍 自动检测结果：
□ 是否为他人项目？是/否
□ 如是，建议执行阶段二（项目私有化）
```

---

## 阶段二：项目私有化

**触发词**：「私有化」「重写README」「替换主题」

**执行时机**：从他人项目改造为自己的项目时

### 🔴 必须按步骤执行

```
[1/6] 检测原始项目信息
      ├─ 读取 README.md
      ├─ 提取原作者名称
      └─ 提取原始仓库地址

[2/6] 输出检测结果，询问用户：
      「检测到原项目：<原项目名称>
        作者：<原作者>
        
        是否进行私有化改造？
        1. 重写 README（中英双语）
        2. 替换主题信息为 Cryodream
        3. 添加原作者鸣谢
        
        是否继续？(y/n)」

[3/6] 用户确认后，备份原始 README：
      cp README.md README.original.md

[4/6] 生成新 README（中英双语）：
      ├─ README.md（中文）
      └─ README.en.md（英文）

[5/6] 更新元信息文件：
      ├─ package.json
      ├─ LICENSE
      └─ 其他配置文件

[6/6] 输出变更报告，询问是否提交
```

### 功能说明

将从远程获取的开源项目改造为符合 Cryodream 规范的私有项目：

- 重写 README（中英双语）
- 替换项目主题信息（名称、作者、GitHub、邮箱）
- 遍历代码替换主题信息
- 添加原作者鸣谢

### 主题信息替换规则

| 替换项 | 替换为 | 替换范围 |
|:-------|:-------|:---------|
| 项目名称 | `Cryodream` 或 `Cryodream - <功能名>` | README, package.json, 配置文件 |
| 作者名称 | `Cryodream` | README, package.json, LICENSE |
| GitHub 地址 | `https://github.com/ice-deep-dream` | README, package.json, 文档文件 |
| 邮箱 | `1172624289@qq.com` | README, package.json, LICENSE |
| Logo/图标 | 移除或替换为 Cryodream 标识 | README, 网页文件 |
| 广告/推广 | 移除 | README, 文档文件 |

### README 模板

#### 中文版 (README.md)

```markdown
# Cryodream - <项目功能名称>

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
cryodream-<project>/
├── src/
│   ├── core/       # 核心模块
│   └── utils/      # 工具函数
└── docs/           # 文档
\`\`\`

---

## 致谢

本项目基于 [原项目名称](原项目地址) 开发，感谢原作者的贡献。

---

## 作者

**Cryodream**
- GitHub: [ice-deep-dream](https://github.com/ice-deep-dream)
- Email: 1172624289@qq.com

---

## 许可证

MIT
```

#### 英文版 (README.en.md)

```markdown
# Cryodream - <Project Feature Name>

<p align="center">
  <img src="https://img.shields.io/badge/Node-18.x-339933?logo=node.js" />
  <img src="https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript" />
  <img src="https://img.shields.io/badge/License-MIT-blue" />
</p>

> One-line project description

---

## Features

| Feature | Description |
|:--------|:------------|
| Feature A | Description |
| Feature B | Description |

---

## Quick Start

\`\`\`bash
# Install
npm install

# Run
npm start
\`\`\`

---

## Directory Structure

\`\`\`
cryodream-<project>/
├── src/
│   ├── core/       # Core module
│   └── utils/      # Utility functions
└── docs/           # Documentation
\`\`\`

---

## Acknowledgments

This project is based on [Original Project Name](Original Project URL). Thanks to the original author's contribution.

---

## Author

**Cryodream**
- GitHub: [ice-deep-dream](https://github.com/ice-deep-dream)
- Email: 1172624289@qq.com

---

## License

MIT
```

### 变更报告模板

```
📋 私有化变更报告
─────────────────────────────────
原始仓库：<原项目地址>

已执行操作：
✅ [1/6] 检测原始项目信息
✅ [2/6] 用户确认私有化
✅ [3/6] 备份原始 README → README.original.md
✅ [4/6] 生成中文版 README.md
✅ [4/6] 生成英文版 README.en.md
✅ [5/6] 更新 package.json 元信息
✅ [5/6] 更新 LICENSE 作者信息
✅ [6/6] 添加致谢章节

替换统计：
├─ 项目名称：X 处
├─ 作者信息：X 处
├─ GitHub 地址：X 处
├─ 邮箱地址：X 处
└─ 移除广告：X 处

保留内容：
├─ 开源协议声明
├─ 核心代码逻辑
└─ 原始文档结构

文件变更：
M  README.md
A  README.en.md
A  README.original.md
M  package.json
M  LICENSE

下一步操作：
□ 检查变更内容
□ 确认无遗漏
□ 执行首次提交
```

---

## 阶段三：提交推送

**触发词**：「提交」「推送」「写commit message」

**执行时机**：有代码变更时

### 🔴 必须按步骤执行

```
[1/5] 查看变更状态：
      git status && git diff --staged

[2/5] 分析变更内容，确定模块范围

[3/5] 生成 commit message，输出给用户确认：
      「建议 commit message: <type>(<module>): <desc>
        是否使用此消息？(y/n/自定义)」

[4/5] 用户确认后执行：
      git add . && git commit -m "<message>"

[5/5] 询问是否推送：
      「是否推送到远程？(y/n)」
```

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

### 模块命名规则

- 按目录/功能划分：`core`, `ui`, `api`, `docs`, `utils`
- 单文件变更：用文件名（去掉扩展名）

---

## 阶段四：生成仓库文档

**触发词**：「生成文档」「初始化仓库文档」「生成 README」

**执行时机**：项目成熟期，需要规范化文档时

### 🔴 必须按步骤执行

```
[1/4] 扫描项目结构，确认技术栈

[2/4] 确认环境版本，输出给用户：
      「检测到技术栈：
        - Node: 18.x
        - Vue: 3.x
        是否生成 README？」

[3/4] 生成中英双语 README

[4/4] 输出文件预览，询问是否保存
```

### 输出要求

- **双语**：中文版主文件，英文版 `README.en.md`
- **Icon**：使用 Shields.io 徽章显示环境
- **风格**：干净优雅，表格对齐，无冗余

### 徽章模板

| 环境 | 徽章 |
|:-----|:-----|
| Node | `https://img.shields.io/badge/Node-18.x-339933?logo=node.js` |
| Python | `https://img.shields.io/badge/Python-3.10-3776AB?logo=python` |
| TypeScript | `https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript` |
| React | `https://img.shields.io/badge/React-18.x-61DAFB?logo=react` |
| Vue | `https://img.shields.io/badge/Vue-3.x-4FC08D?logo=vue.js` |

---

## 阶段五：CHANGELOG 生成

**触发词**：「生成 CHANGELOG」

**执行时机**：版本发布时

### 🔴 必须按步骤执行

```
[1/4] 获取版本标签：git tag --sort=-v:refname

[2/4] 获取提交记录：git log <tag1>..<tag2> --oneline

[3/4] 分类整理为 Added / Fixed / Changed

[4/4] 生成 CHANGELOG.md，询问是否保存
```

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

---

## 阶段六：Issue 管理

**触发词**：「提交 issue」「创建 issue」

**执行时机**：发现问题/改进点时

### 🔴 必须按步骤执行

```
[1/4] 检测 gh CLI 状态：
      where gh && gh auth status

[2/4] 生成 Issue 内容，输出给用户审核

[3/4] 用户确认后执行：
      gh issue create --repo <repo> --title "<title>" --body "<body>"

[4/4] 返回 Issue URL
```

### ⚠️ 推荐机制（重要）

**Skill 不主动提交 Issue**，而是分析后推荐，由用户决定。

```
推荐提交 Issue 的场景：
• 功能缺失，需要规划实现
• 复杂 Bug，需要详细记录
• 架构改进建议
• 性能优化方案
• 文档完善需求

不推荐提交 Issue 的场景：
• 简单 typo 修复
• 一次性配置问题
• 用户操作失误
• 可立即修复的小问题
```

### Issue 模板

| 类型 | 标题前缀 | 内容模板 |
|:-----|:---------|:---------|
| bug | `fix:` | 问题描述、复现步骤、期望结果、实际结果 |
| feature | `feat:` | 功能描述、使用场景、技术方案、验收标准 |
| enhancement | `enhance:` | 现状分析、改进方案、预期收益 |

---

## 阶段七：部署 VitePress 文档站点

**触发词**：「部署文档站点」「创建文档站点」「配置 GitHub Pages」「部署 VitePress」

**执行时机**：需要文档网站时

### 🔴 必须按步骤执行

```
[1/5] 分析项目结构，确认技术栈和功能

[2/5] 输出部署方案，询问用户确认：
      「将创建 VitePress 文档站点：
        - 目录：docs/
        - GitHub Actions 自动部署
        - GitHub Pages 托管
        
        是否继续？」

[3/5] 用户确认后创建：
      ├─ docs/ 目录结构
      ├─ .vitepress/config.ts
      ├─ index.md 首页
      └─ .github/workflows/deploy.yml

[4/5] 提交并推送（询问用户确认）

[5/5] 输出 GitHub Pages 设置步骤
```

### 目录结构

```
project/
├── docs/
│   ├── .vitepress/
│   │   └── config.ts      # VitePress 配置
│   ├── public/            # 静态资源
│   │   └── 404.html       # SPA 路由处理
│   ├── index.md           # 首页
│   ├── guide/             # 指南文档
│   └── package.json       # 文档依赖
└── .github/
    └── workflows/
        └── deploy.yml     # GitHub Actions 部署
```

### ⚠️ 关键配置

| 配置项 | 说明 |
|:-------|:-----|
| `base` | 必须设置为 `/仓库名/`，否则 GitHub Pages 无法正确加载资源 |
| `ignoreDeadLinks` | 设置为 `true` 避免 localhost 链接导致构建失败 |

### GitHub Pages 设置步骤

部署后提醒用户：

1. 进入仓库 → **Settings** → **Pages**
2. 在 **Source** 中选择 **GitHub Actions**
3. 等待部署完成
4. 访问地址：`https://<username>.github.io/<repo-name>/`

---

## 阶段八：Pull Request

**触发词**：「创建 PR」「提交 PR」「新建 PR」

**执行时机**：分支开发完成时

### 🔴 必须按步骤执行

```
[1/5] 检测当前分支：git branch --show-current

[2/5] 推送分支（如未推送）：git push -u origin <branch>

[3/5] 生成 PR 标题和描述，输出给用户审核

[4/5] 用户确认后执行：gh pr create

[5/5] 返回 PR URL
```

---

## ⚠️ 注意事项

1. **必须按步骤执行** — 显示进度 `[1/N]`, `[2/N]` 等
2. **每个步骤都要询问** — 关键操作前必须询问用户确认
3. **不自动执行** — 所有 git 操作等待用户确认
4. **先查状态** — 提交前先 `git status`
5. **简洁描述** — commit message ≤ 50字
6. **模块划分** — 按 功能/目录 确定 scope
7. **自动检测下一步** — 完成一个阶段后，检测是否需要下一阶段
