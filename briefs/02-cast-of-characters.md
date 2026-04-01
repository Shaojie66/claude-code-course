# Module 2: 核心角色们

## 教学弧

- **隐喻：** 把 Claude Code 想象成一个管弦乐团——每个组件就像乐手，有自己的乐器（职责），但需要在指挥（QueryEngine）的协调下一起演奏。
- **开场钩子：** Claude Code 有 515,000 行代码，但可以被分成几个核心「角色」，每个角色只做一件事。认识它们，你就能读懂整个系统。
- **核心洞察：** CLI 负责启动、QueryEngine 负责编排对话、Tools 负责执行具体操作、REPL 负责交互界面。
- **"为什么重要?"：** 当 AI 帮你写代码时出问题，你知道该检查哪个组件——是 AI 的判断错了，还是工具执行失败了？

## 代码片段

File: src/Tool.ts (lines 783-792)
```typescript
export function buildTool<D extends AnyToolDef>(def: D): BuiltTool<D> {
  return {
    ...TOOL_DEFAULTS,
    userFacingName: () => def.name,
    ...def,
  } as BuiltTool<D>
}

const TOOL_DEFAULTS = {
  isEnabled: () => true,
  isConcurrencySafe: (_input?: unknown) => false,
  isReadOnly: (_input?: unknown) => false,
  isDestructive: (_input?: unknown) => false,
  checkPermissions: (
    input: { [key: string]: unknown },
    _ctx?: ToolUseContext,
  ): Promise<PermissionResult> =>
    Promise.resolve({ behavior: 'allow', updatedInput: input }),
  toAutoClassifierInput: (_input?: unknown) => '',
  userFacingName: (_input?: unknown) => '',
}
```

File: src/bootstrap/state.ts (lines 1-30) — Session state management
```typescript
type State = {
  originalCwd: string
  projectRoot: string
  sessionId: SessionId
  isInteractive: boolean
  tokenBudget: {
    inputTokens: number
    outputTokens: number
    totalTokens: number
  }
  // ...
}
```

File: src/services/api/claude.ts (lines 1-30) — API client setup
```typescript
import type { BetaRawMessageStreamEvent } from '@anthropic-ai/sdk/resources/beta/messages/messages.mjs'
import { randomUUID } from 'crypto'
import { getAPIProvider } from 'src/utils/model/providers.js'
// Supports: firstParty (Anthropic direct), bedrock, vertex, foundry
export type APIProvider = 'firstParty' | 'bedrock' | 'vertex' | 'foundry'
```

## 交互元素

- [ ] **代码↔英文翻译** — buildTool 函数，展示如何用「构建器模式」创建工具
- [ ] **Quiz** — 3题，场景：调试问题——AI 写了一个文件但没保存，哪一步出错了？
- [ ] **群聊动画** — Actors: CLI startup / QueryEngine / ToolRegistry / REPL。展示启动流程。
- [ ] **架构图** — 展示 src/ 目录结构：entrypoints/, tools/, services/, screens/, utils/ 各负责什么

## 参考文件

- `references/interactive-elements.md` → Code Translation Blocks, Multiple-Choice Quizzes, Group Chat Animation, Architecture Diagram
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** Module 1 — 什么是 Claude Code
- **下一个模块：** Module 3 — 请求如何流转
- **风格备注：** 继续使用 vermillion accent；Actor 颜色：CLI=actor-1, QueryEngine=actor-2, Tools=actor-3, REPL=actor-4, API=actor-5
