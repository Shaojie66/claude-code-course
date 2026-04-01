# Module 1: Claude Code 是什么？

## 教学弧

- **隐喻：** 想象一个「超级翻译官」——你能用自然语言告诉它你想写什么程序，它就会帮你读代码、写代码、运行代码，就像一个永不疲倦的高级程序员助手。
- **开场钩子：** 你每天都在用 Claude Code 和它对话，但有没有想过——当你输入一句话，Claude Code 内部到底发生了什么？
- **核心洞察：** Claude Code 不只是一个聊天机器人，它是一个完整的工作环境，能读文件、写代码、执行命令、搜索网页，还能调用其他工具。
- **"为什么重要?"：** 理解 Claude Code 的工作原理，能帮助你更有效地「指挥」它——知道该用什么指令，知道它能做什么、不能做什么。

## 代码片段

File: src/entrypoints/cli.tsx (lines 1-17)
```typescript
// Runtime polyfill for bun:bundle (build-time macros)
const feature = (_name: string) => false;
if (typeof globalThis.MACRO === "undefined") {
    (globalThis as any).MACRO = {
        VERSION: "2.1.888",
        BUILD_TIME: new Date().toISOString(),
    };
}
// Build-time constants — normally replaced by Bun bundler at compile time
(globalThis as any).BUILD_TARGET = "external";
(globalThis as any).BUILD_ENV = "production";
(globalThis as any).INTERFACE_TYPE = "stdio";
```

File: src/tools.ts (lines 193-201)
```typescript
export function getAllBaseTools(): Tools {
  return [
    AgentTool,
    TaskOutputTool,
    BashTool,
    GlobTool,
    GrepTool,
    ExitPlanModeV2Tool,
    FileReadTool,
    FileEditTool,
    FileWriteTool,
    WebFetchTool,
    WebSearchTool,
    AskUserQuestionTool,
    SkillTool,
    // ... more tools
  ]
}
```

## 交互元素

- [ ] **代码↔英文翻译** — cli.tsx 的 polyfill 代码，展示工具如何模拟"构建时"特性
- [ ] **Quiz** — 3题，风格：场景选择。测试学员是否能判断 Claude Code 能/不能做什么
- [ ] **群聊动画** — Actors: 用户 → Claude Code → 工具链 → 用户，展示请求流程
- [ ] **数据流动画** — actors: 用户 / Claude Code CLI / Anthropic API / 工具执行器

## 参考文件

- `references/interactive-elements.md` → Multiple-Choice Quizzes, Group Chat Animation, Message Flow Animation
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** 无
- **下一个模块：** 核心角色们 — 介绍 CLI、QueryEngine、REPL、Tools 等主要组件
- **风格备注：** 使用 vermillion (#D94F30) 作为 accent color；Actor 颜色分配：用户=actor-1, Claude Code=actor-2, 工具=actor-3, API=actor-4
