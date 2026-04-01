# Module 6: 内置技能——Claude Code 的超能力

## 教学弧

- **隐喻：** 内置技能就像 Claude Code 的「快捷指令」——输入 `/simplify`，它就会启动一个完整的代码审查流程，包括多个子代理并行工作。这就是 prompt engineering 的力量。
- **开场钩子：** Claude Code 不只是一个聊天机器人——它内置了 `/simplify`（代码审查）、`/batch`（并行工作）、`/loop`（定时任务）等强大的技能，让你用简单的斜杠命令触发复杂的 AI 工作流。
- **核心洞察：** 这些「技能」本质上是预设的 system prompt + 多代理编排。理解它们，你就能更好地「指挥」AI，甚至自己构建新的技能。
- **"为什么重要?"：** 知道内置技能的存在和原理，能让你的 AI 协作效率提升 10 倍。

## 代码片段

File: src/skills/bundled/simplify.ts (lines 1-35) — /simplify skill
```typescript
const SIMPLIFY_PROMPT = `# Simplify: Code Review and Cleanup

Review all changed files for reuse, quality, and efficiency. Fix any issues found.

## Phase 1: Identify Changes
Run \`git diff\` to see what changed.

## Phase 2: Launch Three Review Agents in Parallel
Use the AgentTool to launch all three agents concurrently:

### Agent 1: Code Reuse Review
- Search for existing utilities that could replace newly written code
- Flag duplicate functionality

### Agent 2: Code Quality Review
- Redundant state, parameter sprawl, copy-paste patterns

### Agent 3: Efficiency Review
- Algorithmic efficiency, unnecessary complexity
```

File: src/skills/bundled/loop.ts (lines 1-25) — /loop skill
```typescript
const USAGE_MESSAGE = `Usage: /loop [interval] <prompt>

Run a prompt or slash command on a recurring interval.
Intervals: Ns, Nm, Nh, Nd (e.g. 5m, 30m, 2h, 1d).

Examples:
  /loop 5m /babysit-prs
  /loop 30m check the deploy
  /loop check the deploy          (defaults to 10m)`
```

File: src/tools/AgentTool/builtInAgents.ts (lines 22-50) — Built-in agents
```typescript
export function getBuiltInAgents(): AgentDefinition[] {
  return [
    CLAUDE_CODE_GUIDE_AGENT,   // 引导新用户
    EXPLORE_AGENT,              // 探索代码库
    GENERAL_PURPOSE_AGENT,      // 通用任务
    PLAN_AGENT,                 // 计划模式代理
    VERIFICATION_AGENT,          // 验证代理
    STATUSLINE_SETUP_AGENT,     // 状态栏设置
  ]
}
```

## 交互元素

- [ ] **代码↔英文翻译** — simplify.ts 的技能定义，展示如何用 prompt 构建复杂行为
- [ ] **Quiz** — 3题，应用场景：你想让 Claude Code 每小时自动检查部署状态，应该用哪个技能？
- [ ] **群聊动画** — Actors: 用户 / SkillTool / AgentTool (x3) / 工具执行器，展示 /simplify 如何并行启动 3 个审查代理
- [ ] **技能卡片** — 展示 /batch, /loop, /verify, /remember, /claudeInChrome 等内置技能的图标和描述

## 参考文件

- `references/interactive-elements.md` → Code Translation Blocks, Multiple-Choice Quizzes, Group Chat Animation, Pattern/Feature Cards
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** Module 5 — 安全系统
- **下一个模块：** 无
- **风格备注：** 继续使用 vermillion accent；技能用 color-actor-2 (teal)
