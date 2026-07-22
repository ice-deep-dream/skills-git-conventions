---
name: git-conventions
description: Git 规范助手 — 初始化项目、模块提交推送、生成仓库文档（README/CHANGELOG）、Issue 管理、部署 VitePress 文档站点。触发词：初始化项目、连接git、提交推送、生成文档、写commit message、提交issue、部署文档站点。
---

# Git 规范助手

简化版 Git 工作流 + 文档生成 + Issue 管理 + VitePress 文档站点部署。

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

## 功能阶段总览

| 阶段 | 功能 | 触发词 | 执行时机 |
|:----:|:-----|:-------|:---------|
| **阶段一** | 项目初始化 | `初始化项目` `连接git` | 首次使用 skill / 新项目 |
| **阶段二** | 项目私有化 | `私有化` `重写README` | 从他人项目改造时 |
| **阶段三** | 提交推送 | `提交` `推送` `写commit message` | 有代码变更时 |
| **阶段四** | 生成仓库文档 | `生成文档` `生成README` | 项目成熟期 |
| **阶段五** | CHANGELOG 生成 | `生成CHANGELOG` | 版本发布时 |
| **阶段六** | Issue 管理 | `提交issue` `创建issue` | 发现问题/改进点时 |
| **阶段七** | 部署文档站点 | `部署文档站点` `部署VitePress` | 需要文档网站时 |
| **阶段八** | Pull Request | `创建PR` `提交PR` | 分支开发完成时 |

---

## 阶段一：项目初始化

**触发词**：「初始化项目」「连接git」

**执行时机**：首次使用 skill / 新建项目

### 工作流

```
1. 检测 Git 状态
   ├─ 检测是否已初始化 → git rev-parse --git-dir
   ├─ 检测是否有远程 → git remote -v
   └─ 检测当前分支 → git branch --show-current

2. 判断场景
   ├─ 场景 A：无 Git → 执行完整初始化
   ├─ 场景 B：有 Git 无远程 → 提醒用户连接远程
   └─ 场景 C：有 Git 有远程 → 跳过初始化

3. 执行初始化（按场景）
   ├─ git init（如未初始化）
   ├─ 创建 .gitignore（如不存在）
   ├─ git remote add origin <url>（如无远程）
   └─ 首次提交（如无提交记录）

4. 输出状态报告
```

### 场景判断逻辑

#### 场景 A：无 Git 初始化

```
检测：git rev-parse --git-dir 失败

执行：
1. 询问用户项目目的
2. git init
3. 创建 .gitignore（按项目类型）
4. 提醒用户连接远程
5. 等待用户确认后执行首次提交
```

#### 场景 B：有 Git 无远程

```
检测：git remote -v 为空

执行：
1. 输出警告：「⚠️ 当前项目未连接远程仓库」
2. 询问用户是否连接远程
3. 用户输入远程地址 → git remote add origin <url>
4. 检测是否需要推送 → git push -u origin <branch>
```

#### 场景 C：有 Git 有远程

```
检测：git remote -v 有内容

执行：
1. 输出当前状态：「✅ 项目已初始化并连接远程」
2. 显示远程地址和当前分支
3. 询问用户是否需要其他操作
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
```

---

## 阶段二：项目私有化

**触发词**：「私有化」「重写README」「替换主题」

**执行时机**：从他人项目改造为自己的项目时

### 功能说明

将从远程获取的开源项目改造为符合 Cryodream 规范的私有项目：

- 重写 README（中英双语）
- 替换项目主题信息（名称、作者、GitHub、邮箱）
- 遍历代码替换主题信息
- 添加原作者鸣谢

### 工作流

```
1. 检测远程仓库
   ├─ git remote -v → 获取原始仓库地址
   └─ 确认是否为他人项目

2. 询问用户确认
   ├─ 是否进行私有化改造？
   ├─ 是否保留原项目结构？
   └─ 是否需要额外定制？

3. 执行私有化
   ├─ 备份原始 README（可选）
   ├─ 生成新 README（中英双语）
   ├─ 遍历代码替换主题信息
   └─ 添加鸣谢信息

4. 输出变更报告
```

### README 重写规范

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

### 主题信息替换规则

| 替换项 | 替换为 | 替换范围 |
|:-------|:-------|:---------|
| 项目名称 | `Cryodream` 或 `Cryodream - <功能名>` | README, package.json, 配置文件 |
| 作者名称 | `Cryodream` | README, package.json, LICENSE |
| GitHub 地址 | `https://github.com/ice-deep-dream` | README, package.json, 文档文件 |
| 邮箱 | `1172624289@qq.com` | README, package.json, LICENSE |
| Logo/图标 | 移除或替换为 Cryodream 标识 | README, 网页文件 |
| 广告/推广 | 移除 | README, 文档文件 |

### 代码遍历替换策略

```
遍历范围：
├─ README.md / README.en.md
├─ package.json
├─ LICENSE
├─ docs/ 目录
├─ src/ 目录（可选，需用户确认）
└─ 配置文件（.env.example, config.*）

替换原则：
├─ 仅替换用户可见的元信息
├─ 不修改核心业务逻辑代码
├─ 保留原有的开源协议声明
└─ 添加鸣谢信息
```

### 鸣谢模板

```markdown
## 致谢 / Acknowledgments

本项目基于 [原项目名称](原项目地址) 开发。

感谢原作者：
- 原作者名称 (原GitHub地址)

本项目在原有基础上进行了以下改造：
- 重写了项目文档
- 调整了项目结构
- 替换了主题配置

---

## 作者 / Author

**Cryodream**
- GitHub: [ice-deep-dream](https://github.com/ice-deep-dream)
- Email: 1172624289@qq.com
```

### 变更报告模板

```
📋 私有化变更报告
─────────────────────────────────
原始仓库：<原项目地址>

已执行操作：
✅ 备份原始 README → README.original.md
✅ 生成中文版 README.md
✅ 生成英文版 README.en.md
✅ 更新 package.json 元信息
✅ 更新 LICENSE 作者信息
✅ 添加致谢章节

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

下一步操作：
□ 检查变更内容
□ 确认无遗漏
□ 提交变更
```

### 使用示例

```
用户: 我从 GitHub 克隆了一个项目，想把它改造成自己的项目

AI: 检测到当前项目来自：<原项目地址>
    
    是否需要进行私有化改造？
    1. 重写 README（中英双语）
    2. 替换主题信息为 Cryodream
    3. 添加原作者鸣谢
    
    是否继续？(y/n)

用户: y

AI: 正在执行私有化改造...
    
    [1/5] 备份原始 README...
    [2/5] 生成新 README...
    [3/5] 替换主题信息...
    [4/5] 更新元信息文件...
    [5/5] 添加鸣谢信息...
    
    ✅ 私有化完成！
    
    📋 变更报告：
    ─────────────────────────────────
    已替换 15 处项目信息
    已移除 3 处广告链接
    已添加致谢章节
    
    文件变更：
    M  README.md
    A  README.en.md
    A  README.original.md
    M  package.json
    M  LICENSE
    
    是否需要查看详细变更？(y/n)

用户: n

AI: 建议执行以下操作：
    1. 检查 README 内容是否符合预期
    2. 确认 package.json 配置正确
    3. 提交变更：git add . && git commit -m "chore: privatize project"
```

---

## 阶段三：提交推送

**触发词**：「提交」「推送」「写commit message」

**执行时机**：有代码变更时

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

## 阶段四：生成仓库文档

**触发词**：「生成文档」「初始化仓库文档」「生成 README」

**执行时机**：项目成熟期，需要规范化文档时

### 输出要求

- **双语**：中文版主文件，英文版 `README.en.md`
- **Icon**：使用 Shields.io 徽章显示环境
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

| 环境 | 徽章 |
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

## 阶段五：CHANGELOG 生成

**触发词**：「生成 CHANGELOG」

**执行时机**：版本发布时

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

## 阶段六：Issue 管理

**触发词**：「提交 issue」「创建 issue」

**执行时机**：发现问题/改进点时

### 推荐机制

**重要**：Skill 不主动提交 Issue，而是**分析后推荐**，由用户决定。

```
触发场景：
1. 用户明确要求 → 「提交 issue」「创建 issue」
2. AI 发现难点/待改进点 → 推荐提交，等待用户确认

禁止：
- 简单问题不推荐提交 Issue
- 不经用户确认直接创建 Issue
```

### 前置检测

```
1. 检测 gh CLI → where gh
2. 检测登录状态 → gh auth status
3. 未登录提示 → 请先运行 gh auth login
```

### Issue 推荐流程

```
AI 发现潜在问题
    ↓
评估是否值得提交 Issue
    ↓
┌─────────────────────────────────────┐
│ 推荐提交 Issue 的场景：              │
│ • 功能缺失，需要规划实现              │
│ • 复杂 Bug，需要详细记录              │
│ • 架构改进建议                        │
│ • 性能优化方案                        │
│ • 文档完善需求                        │
├─────────────────────────────────────┤
│ 不推荐提交 Issue 的场景：             │
│ • 简单 typo 修复                      │
│ • 一次性配置问题                      │
│ • 用户操作失误                        │
│ • 可立即修复的小问题                  │
└─────────────────────────────────────┘
    ↓
推荐格式：
「我发现以下问题可能需要记录 Issue：
  1. [问题描述]
  2. [问题描述]
  是否需要创建 Issue？(y/n/选择)」
    ↓
用户确认 → 选择要提交的问题
    ↓
生成 Issue 内容 → 用户审核
    ↓
确认创建 → gh issue create
```

### Issue 模板

| 类型 | 标题前缀 | 内容模板 |
|:-----|:---------|:---------|
| bug | `fix:` | 问题描述、复现步骤、期望结果、实际结果 |
| feature | `feat:` | 功能描述、使用场景、技术方案、验收标准 |
| enhancement | `enhance:` | 现状分析、改进方案、预期收益 |

### 工作流

```
用户确认提交后：

1. 确认仓库 → git remote -v 或用户指定
2. 生成标题 → AI 根据分析生成
3. 生成内容 → 包含背景、描述、方案、验收标准
4. 用户审核 → 展示完整内容，等待修改确认
5. 执行创建 → gh issue create --repo <repo> --title "<title>" --body "<body>"
6. 返回链接 → 输出 Issue URL
```

### 使用示例

```
AI: 我分析了代码，发现以下问题值得记录 Issue：
    1. 进程监控缺少错误处理（可能导致崩溃）
    2. 配置文件迁移没有版本兼容（升级后配置丢失）
    
    是否需要创建 Issue？
    a) 全部创建
    b) 只创建 #1
    c) 只创建 #2
    d) 暂不创建

用户: a

AI: 正在生成 Issue 内容...

    Issue #1: fix: 进程监控添加错误处理
    ─────────────────────────────
    ## 问题描述
    ProcessMonitor 在枚举进程时没有错误处理...
    
    ## 建议方案
    添加 try-catch 和错误日志...
    
    ## 验收标准
    - [ ] 添加错误处理
    - [ ] 添加单元测试
    ─────────────────────────────
    
    确认创建？

用户: y

AI: ✅ Issue 已创建: https://github.com/xxx/xxx/issues/1
```

---

## 阶段七：部署 VitePress 文档站点

**触发词**：「部署文档站点」「创建文档站点」「配置 GitHub Pages」「部署 VitePress」

**执行时机**：需要文档网站时

### 功能说明

为任何项目创建 VitePress 文档站点，并配置 GitHub Actions 自动部署到 GitHub Pages。

### 工作流

```
1. 分析项目结构 → 确认技术栈和功能
2. 创建 docs/ 目录 → 生成 VitePress 站点结构
3. 配置 VitePress → config.ts、首页、文档
4. 创建 GitHub Actions → 自动部署 workflow
5. 提交并推送 → 等待用户确认
6. 提醒启用 Pages → 用户手动设置
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

### package.json 模板

```json
{
  "name": "<project>-docs",
  "version": "0.1.0",
  "description": "<project> 文档",
  "type": "module",
  "scripts": {
    "docs:dev": "vitepress dev",
    "docs:build": "vitepress build",
    "docs:preview": "vitepress preview"
  },
  "devDependencies": {
    "vitepress": "^1.6.4"
  }
}
```

### config.ts 关键配置

```typescript
import { defineConfig } from 'vitepress'

export default defineConfig({
  title: '<Project Name>',
  description: '<Project Description>',
  base: '/<repo-name>/',        // 必须与仓库名一致
  ignoreDeadLinks: true,        // 忽略 localhost 等死链接
  themeConfig: {
    nav: [...],
    sidebar: [...],
    socialLinks: [
      { icon: 'github', link: 'https://github.com/<user>/<repo>' },
    ],
    search: {
      provider: 'local',
    },
  },
})
```

### GitHub Actions workflow 模板

```yaml
name: Deploy VitePress site to Pages

on:
  push:
    branches: [master]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true
    steps:
      - name: Checkout
        uses: actions/checkout@v5
        with:
          fetch-depth: 0

      - name: Setup Node
        uses: actions/setup-node@v6
        with:
          node-version: 24
          cache: npm
          cache-dependency-path: docs/package-lock.json

      - name: Setup Pages
        uses: actions/configure-pages@v5

      - name: Install dependencies
        run: npm ci
        working-directory: docs

      - name: Build with VitePress
        run: npm run docs:build
        working-directory: docs

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/.vitepress/dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: build
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 404.html 模板

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Project Name</title>
  <script>
    sessionStorage.redirect = location.href;
  </script>
  <meta http-equiv="refresh" content="0;URL='/repo-name/'"></meta>
</head>
<body>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</body>
</html>
```

### 重要注意事项

| 配置项 | 说明 |
|:-------|:-----|
| `base` | 必须设置为 `/仓库名/`，否则 GitHub Pages 无法正确加载资源 |
| `ignoreDeadLinks` | 设置为 `true` 避免 localhost 链接导致构建失败 |
| `FORCE_JAVASCRIPT_ACTIONS_TO_NODE24` | 强制使用 Node.js 24 运行 Actions |
| `actions/checkout@v5` | 最新版本 checkout action |
| `actions/setup-node@v6` | 最新版本 node setup |

### GitHub Pages 设置提醒

部署后提醒用户：

1. 进入仓库 → **Settings** → **Pages**
2. 在 **Source** 中选择 **GitHub Actions**（不是 Deploy from a branch）
3. 等待部署完成（约 2-5 分钟）
4. 访问地址：`https://<username>.github.io/<repo-name>/`

### 常见问题处理

| 问题 | 解决方案 |
|:-----|:---------|
| 构建失败：dead links | 在 config.ts 中添加 `ignoreDeadLinks: true` |
| 页面 404 | 确认 `base` 设置为 `/仓库名/`，等待 Pages 生效 |
| Actions Node.js 20 警告 | 添加 `FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true` |

---

## 阶段八：Pull Request

**触发词**：「创建 PR」「提交 PR」「新建 PR」

**执行时机**：分支开发完成时

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
6. **VitePress 部署** — base 路径必须与仓库名一致
7. **用户确认** — 部署文档站点前需用户确认
8. **私有化谨慎** — 替换主题信息前需用户确认，保留原作者鸣谢
