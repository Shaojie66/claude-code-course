# Module 3: 请求的旅程——从输入到响应

## 教学弧

- **隐喻：** 就像点外卖——你下订单（输入），外卖平台接收（CLI处理），厨房准备（AI模型思考），骑手送餐（工具执行），最后你收到食物（响应）。每一步都可能出问题，了解流程才能调试。
- **开场钩子：** 当你在 Claude Code 里输入「帮我读这个文件」，这简单的一句话背后，经历了一个精心编排的五步舞蹈。
- **核心洞察：** Claude Code 的请求处理是一个「循环」——用户输入 → API 调用 → 工具执行 → 结果返回 → 再次调用 API，直到任务完成。
- **"为什么重要?"：** 当 Claude Code 做了一件你没预期的事，知道请求在哪个环节「拐错了弯」，能帮你快速修复提示词或配置。

## 代码片段

File: src/query.ts (lines 1-30) — Core query function signature
```typescript
export async function query(
  params: QueryParams,
): Promise<QueryResult> {
  const { messages, systemPrompt, tools, tool_choice, maxTokens } = params

  // 1. Build API request
  const request = buildClaudeRequest({
    messages,
    systemPrompt,
    tools,
    tool_choice,
    maxTokens,
  })

  // 2. Stream response from API
  const stream = await callClaudeAPI(request)

  // 3. Process streaming events
  for await (const event of stream) {
    if (event.type === 'content_block_delta') {
      // Handle text delta
      yield { type: 'text', content: event.delta.text }
    } else if (event.type === 'tool_use') {
      // 4. Execute tool
      const result = await executeTool(event.tool_use)
      // 5. Add result back to messages for next turn
      messages.push(createToolResultMessage(event.tool_use.id, result))
    }
  }
}
```

File: src/utils/permissions/PermissionMode.ts (lines 16-38) — Permission modes
```typescript
export const EXTERNAL_PERMISSION_MODES = [
  'acceptEdits',
  'bypassPermissions',
  'default',
  'dontAsk',
  'plan',
] as const

export type ExternalPermissionMode = (typeof EXTERNAL_PERMISSION_MODES)[number]

export type InternalPermissionMode = ExternalPermissionMode | 'auto' | 'bubble'
export type PermissionMode = InternalPermissionMode
```

## 交互元素

- [ ] **代码↔英文翻译** — query.ts 的伪代码流程，解释流式 API 处理
- [ ] **Quiz** — 3题，调试场景：Claude Code 说「无法读取文件」，问题出在流程的哪一步？
- [ ] **群聊动画** — Actors: 用户 / CLI输入处理器 / API客户端 / 工具执行器 / 用户输出。5个消息，展示完整循环。
- [ ] **数据流动画** — steps: 用户输入 → 解析命令 → API调用 → 流式响应 → 工具调用 → 结果 → 下一轮API → 最终响应

## 参考文件

- `references/interactive-elements.md` → Code Translation Blocks, Multiple-Choice Quizzes, Group Chat Animation, Message Flow Animation
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** Module 2 — 核心角色们
- **下一个模块：** Module 4 — 工具系统详解
- **风格备注：** 继续使用 vermillion accent；数据流动画用 actor-1=用户, actor-2=CLI, actor-3=API, actor-4=工具
