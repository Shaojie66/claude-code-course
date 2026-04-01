# Module 5: 安全系统——Claude Code 如何保护你

## 教学弧

- **隐喻：** 权限系统就像 Claude Code 的「安保部门」——每个危险操作都要过安检，检票员（权限系统）会问：「你确定要运行这个命令吗？这个命令可能会删掉你的文件。」
- **开场钩子：** Claude Code 可以运行任意 shell 命令、读写文件、甚至执行 curl 下载脚本——它怎么知道你不是被一个恶意提示词欺骗了？
- **核心洞察：** Claude Code 有多层安全机制：权限模式（plan/auto/default）、危险命令检测（YOLO 分类器）、路径验证、shell 语法分析。
- **"为什么重要?"：** 知道 Claude Code 的安全机制，你就能理解为什么某些操作被阻止，以及如何在安全的前提下给它更高的自由度。

## 代码片段

File: src/utils/permissions/dangerousPatterns.ts (lines 18-50) — Dangerous patterns
```typescript
export const CROSS_PLATFORM_CODE_EXEC = [
  'python', 'python3', 'node', 'deno', 'ruby',
  'perl', 'php', 'lua',
  'npx', 'bunx', 'npm run', 'yarn run', 'pnpm run', 'bun run',
  'bash', 'sh', 'ssh',
] as const

export const DANGEROUS_BASH_PATTERNS: readonly string[] = [
  ...CROSS_PLATFORM_CODE_EXEC,
  'zsh', 'fish', 'eval', 'exec', 'env',
  // 网络危险命令
  'curl', 'wget', 'nc', 'netcat',
  // 文件系统危险命令
  'chmod', 'chown', 'sudo', 'su', 'dd', 'mkfs',
]
```

File: src/types/permissions.ts (lines 16-38) — Permission modes
```typescript
export const EXTERNAL_PERMISSION_MODES = [
  'acceptEdits',      // 自动接受所有编辑
  'bypassPermissions', // 跳过所有权限检查（危险！）
  'default',          // 默认，每次询问
  'dontAsk',          // 不询问，直接拒绝
  'plan',             // 计划模式，只读分析
] as const

export type PermissionMode = ExternalPermissionMode | 'auto' | 'bubble'

// 权限行为
export type PermissionBehavior = 'allow' | 'deny' | 'ask'
```

File: src/utils/permissions/yoloClassifier.ts — YOLO (fast path) classifier
```typescript
// YOLO 模式：跳过复杂的权限检查，直接放行"安全"的命令
// 用于 /auto 模式下，当命令被分类为低风险时的快速路径
export function classifyAsYolo(input: string): boolean {
  // 检查命令是否只包含"安全"的只读操作
  // 比如 `ls`, `cat` 小文件, `git status` 等
}
```

## 交互元素

- [ ] **代码↔英文翻译** — DANGEROUS_BASH_PATTERNS 列表，解释为什么这些命令被标记为危险
- [ ] **Quiz** — 3题，调试场景：你给 Claude Code 一个提示词让它「清理 node_modules」，它应该允许执行吗？
- [ ] **群聊动画** — Actors: 用户 / CLI / 权限系统 / BashTool / 用户，展示权限检查流程
- [ ] **权限/配置徽章** — 展示 5 种权限模式及其风险等级

## 参考文件

- `references/interactive-elements.md` → Code Translation Blocks, Multiple-Choice Quizzes, Group Chat Animation, Permission/Config Badges
- `references/design-system.md` → 全部 tokens
- `references/content-philosophy.md` → 全部原则
- `references/gotchas.md` → 全部检查项

## 连接

- **前置模块：** Module 4 — 工具系统
- **下一个模块：** Module 6 — 内置技能与扩展
- **风格备注：** 继续使用 vermillion accent；安全相关用 color-warning (red tones)
