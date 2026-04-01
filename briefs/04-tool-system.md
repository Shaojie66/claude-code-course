# Module 4: 工具系统——Claude Code 的瑞士军刀

## 教学弧

- **隐喻：** 工具就像 Claude Code 的「技能包」——每学会一个新技能，就能做更多事。BashTool 让它能跑命令，FileReadTool 让它能读书籍，WebSearchTool 让它能查资料。
- **开场钩子：** Claude Code 能读文件、写代码、运行程序、搜索网页——这些能力不是内置的，而是通过「工具」扩展的。理解工具，就是理解 Claude Code 的能力边界。
- **核心洞察：** 40+ 工具，每个工具都是一个独立的 TypeScript 模块，通过 `buildTool()` 统一接口注册到系统。
- **"为什么重要?"：** 当你想让 Claude Code 做某件事，先看看它有没有对应的工具——知道工具的存在，就能提出更好的要求。

## 代码片段

File: src/tools.ts (lines 193-250) — Complete tool list
```typescript
export function getAllBaseTools(): Tools {
  return [
    AgentTool,           // 运行子代理
    TaskOutputTool,      // 读取后台任务输出
    BashTool,            // 执行 shell 命令
    GlobTool,            // 文件名匹配搜索
    GrepTool,            // 内容搜索
    ExitPlanModeV2Tool,  // 退出计划模式
    FileReadTool,        // 读取文件
    FileEditTool,        // 编辑文件
    FileWriteTool,       // 写入文件
    NotebookEditTool,    // Jupyter notebook 编辑
    WebFetchTool,        // 获取网页内容
    WebSearchTool,       // 搜索网页
    TodoWriteTool,       // 写 todo 列表
    TaskStopTool,        // 停止后台任务
    AskUserQuestionTool, // 向用户提问
    SkillTool,           // 调用技能
    EnterPlanModeTool,   // 进入计划模式
    BriefTool,           // 简短模式
    ListMcpResourcesTool,// 列出 MCP 资源
    ReadMcpResourceTool,// 读取 MCP 资源
    ToolSearchTool,      // 工具搜索
    // 条件加载的工具...
    // CronCreate/Delete/List — 定时任务
    // PowerShellTool — Windows PowerShell
    // ... 总计 40+ 工具
  ]
}
```

File: src/tools/BashTool/bashSecurity.ts (lines 1-40) — Dangerous pattern detection
```typescript
const DANGEROUS_BASH_PATTERNS: readonly string[] = [
  'python', 'python3', 'node', 'deno', 'ruby',
  'perl', 'php', 'lua', 'npx', 'bunx',
  'npm run', 'yarn run', 'pnpm run', 'bun run',
  'bash', 'sh', 'zsh', 'fish', 'eval', 'exec', 'env',
  'ssh', 'curl', 'wget', 'nc', 'netcat',
  'chmod', 'chown', 'sudo', 'su',
  'dd', 'mkfs', 'fdisk', 'parted',
]
// 检测命令替换、进程替换等危险语法
const COMMAND_SUBSTITUTION_PATTERNS = [
  { pattern: /<\(/, message: 'process substitution <()' },
  { pattern: />\(/, message: 'process substitution >()' },
  { pattern: /\$\(/, message: '$() command substitution' },
  { pattern: /\$\{/, message: '${} parameter substitution' },
]
```

## 交互元素

- [ ] **代码↔英文翻译** — getAllBaseTools() 列表，解释每个工具的用途
- [ ] **Quiz** — 3题，场景：你想让 Claude Code 帮你部署代码到服务器，它需要什么工具？它有权做这件事吗？
- [ ] **群聊动画** — Actors: 用户 / SkillTool / AgentTool / BashTool / FileEditTool，展示如何组合工具
- [ ] **拖拽匹配** — 将工具名称拖拽到对应功能描述

## 参考文件

- `references/interactive-elements.md` → Code Translation Blocks, Multiple-Choice Quizzes, Group Chat Animation, Drag-and-Drop
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** Module 3 — 请求如何流转
- **下一个模块：** Module 5 — 安全与权限系统
- **风格备注：** 继续使用 vermillion accent；工具用 color-actor-3 (teal)
