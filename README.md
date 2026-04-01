# Claude Code 逆向工程

深入理解 Claude Code 内部原理的交互式课程。专为「氛围编码者」设计——用自然语言指挥 AI 写代码、想理解底层机制的人。

**[开始学习 →](https://shaojie66.github.io/claude-code-course/)**

---

## 你会学到什么

Claude Code 有 515,000 行代码，但这门课帮你拆解它的核心逻辑。

| 模块 | 内容 |
|------|------|
| 01 · Claude Code 是什么 | 整体架构、CLI 入口、与传统 IDE 的区别 |
| 02 · 核心角色们 | CLI、QueryEngine、Tools、REPL 如何协作 |
| 03 · 请求的旅程 | 从输入到响应的五步流程 |
| 04 · 工具系统 | 40+ 内置工具的注册与执行机制 |
| 05 · 安全系统 | 权限模式、危险命令检测、YOLO 分类器 |
| 06 · 内置技能 | `/simplify`、`/batch`、`/loop` 的实现原理 |

学完这门课，你会对 Claude Code 有「驾驭感」——知道问题出在哪里，知道该怎么问它。

---

## 课程特色

- **交互式动画** — 群聊动画、数据流动画、代码翻译块
- **真实源码** — 直接分析 Claude Code 的 TypeScript 源码
- **调试场景** — 每节结尾的 Quiz 训练故障诊断思维
- **精密机器揭秘感** — 冷静、专业、满足好奇心

---

## 项目结构

```
claude-code-course/
├── index.html          # 编译后的课程页面
├── build.sh            # 构建脚本
├── styles.css          # 设计系统 (暖白 + vermillion)
├── main.js             # 交互逻辑 (动画、Quiz)
├── modules/            # 课程模块 HTML 源文件
│   ├── 01-what-is-claude-code.html
│   ├── 02-cast-of-characters.html
│   ├── 03-request-flow.html
│   ├── 04-tool-system.html
│   ├── 05-security-system.html
│   └── 06-bundled-skills.html
├── briefs/             # 模块设计文档
└── _base.html          # 页面壳
```

**构建方式：**

```bash
bash build.sh
```

---

## 设计系统

| Token | Value | 用途 |
|-------|-------|------|
| Background | `#FAF7F2` | 暖白背景 |
| Accent | `#D94F30` | Vermillion 强调 |
| Font | Bricolage Grotesque | 标题 |
| Font | DM Sans | 正文 |
| Font | JetBrains Mono | 代码 |

无 AI 色板、无 glassmorphism、左对齐布局。

---

## 访问课程

**在线：** https://shaojie66.github.io/claude-code-course/

**本地开发：**

```bash
# 克隆后直接打开
open index.html

# 或用本地服务器
python3 -m http.server 8000
# 访问 http://localhost:8000
```

---

## 关于这门课

**目标用户：** 独立开发者、设计师、产品经理——用 AI 工具构建产品，想理解 AI 的「内部结构」来更有效地指挥它。

**不教什么：** 不教编程基础，不教如何使用 Claude Code（那是官方文档的事）。

**教什么：** 当 Claude Code 做了一件你没预期的事，你知道请求在哪个环节「拐错了弯」，能快速修复提示词或配置。

---

*课程内容基于 Claude Code 公开源码分析，适合已具备编程基础的学习者。*
