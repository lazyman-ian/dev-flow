# dev-flow Plugin v3.6

统一的开发工作流: planning → coding → commit → PR → release

> Author: lazyman

## 特性

- **完整工作流**: 从计划到发布的全流程自动化
- **智能自动化**: 自动推断 scope、生成 commit message、PR 描述
- **状态持久化**: Ledger 跨 session 保持、Reasoning 记录决策历史
- **质量保障**: 自动 `make fix` + `make check`
- **平台支持**: iOS (Swift), Android (Kotlin), Web (TypeScript)
- **自我迭代**: `/dev-flow:meta-iterate` 分析 session 表现，持续优化 agent/skill prompt

## 安装

### 从本地目录

```bash
# 1. 添加本地目录为插件源
/plugins add-marketplace dev-flow-local --directory /path/to/dev-flow-plugin

# 2. 安装插件
/plugins add dev-flow@dev-flow-local
```

### 从 GitHub 仓库

```bash
# 1. 添加 GitHub 仓库为插件源
/plugins add-marketplace lazyman-ian --github lazyman-ian/dev-flow

# 2. 安装插件
/plugins add dev-flow@lazyman-ian
```

验证: 输入 `/dev-flow:dev` 查看开发状态。

## 命令概览

### 核心流程

| 命令 | 功能 | 自动化 |
|------|------|--------|
| `/dev-flow:dev` | 状态 + 下一步建议 | 阶段检测、错误提示 |
| `/dev-flow:start` | 开始任务 | 创建分支、Ledger |
| `/dev-flow:plan` | 创建计划 | 研究、设计、迭代 |
| `/dev-flow:validate` | 验证计划 | 技术选型检查 |
| `/dev-flow:implement` | 执行计划 | TDD + Agent 编排 |
| `/dev-flow:commit` | 提交代码 | make fix、scope 推断、reasoning |
| `/dev-flow:pr` | 创建 PR | 推送、描述、代码审查 |
| `/dev-flow:release` | 发布版本 | 版本号、changelog |

### 辅助功能

| 命令 | 功能 |
|------|------|
| `/dev-flow:ledger` | 状态账本管理 |
| `/dev-flow:recall` | 搜索历史决策 |
| `/dev-flow:describe` | 详细 PR 描述 |
| `/dev-flow:tokens` | Token 使用分析 |
| `/dev-flow:deps` | 依赖检查 |
| `/dev-flow:switch` | 智能分支切换 |
| `/dev-flow:cleanup` | 清理合并分支 |

### 自我迭代

| 命令 | 功能 |
|------|------|
| `/dev-flow:meta-iterate` | 完整 5 阶段迭代流程 |
| `/dev-flow:meta-iterate evaluate` | 评估 session 表现 |
| `/dev-flow:meta-iterate diagnose` | 诊断问题根因 |
| `/dev-flow:meta-iterate propose` | 生成改进提案 |
| `/dev-flow:meta-iterate apply` | 应用改进 (需人工审核) |
| `/dev-flow:meta-iterate verify` | 验证改进效果 |

## 工作流

```
┌─────────────────────────────────────────────────────────┐
│                  /dev-flow:start                        │
│              创建分支 + Ledger                          │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                /dev-flow:plan (可选)                    │
│           研究 → 设计 → 生成实现计划                    │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│              /dev-flow:validate (可选)                  │
│              验证技术选型 (2024-2025)                   │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                 /dev-flow:implement                     │
│              TDD (Red-Green-Refactor)                   │
│              Agent 编排 (大任务)                        │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                  /dev-flow:commit                       │
│     make fix → make check → commit → reasoning          │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                    /dev-flow:pr                         │
│       push → 生成描述 (中文) → 代码审查                 │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│                  /dev-flow:release                      │
│          版本建议 → Tag → Release Notes                 │
└─────────────────────────────────────────────────────────┘
```

## Meta-Iterate 自我迭代

持续分析 session 表现，自动迭代改进 agent/skill/rule prompt。

### 5 阶段流程

```
┌─────────────────────────────────────────────────────────┐
│ Phase 1: EVALUATE                                       │
│ 输入: Braintrust session logs                           │
│ 输出: thoughts/evaluations/EVAL-<date>.json             │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Phase 2: DIAGNOSE                                       │
│ 输入: 评估结果                                          │
│ 输出: thoughts/diagnoses/DIAG-<date>.md                 │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Phase 3: PROPOSE                                        │
│ 输入: 诊断报告                                          │
│ 输出: thoughts/proposals/PROP-<date>.md                 │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Phase 4: APPLY (人工审核)                               │
│ 输入: 提案 + 用户确认                                   │
│ 输出: 更新的组件文件 + thoughts/iterations/ITER-NNN.md  │
└─────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────┐
│ Phase 5: VERIFY (需要新 sessions)                       │
│ 输入: 迭代记录 + 新 session 数据                        │
│ 输出: 验证报告                                          │
└─────────────────────────────────────────────────────────┘
```

### 使用示例

```bash
# 完整流程
/dev-flow:meta-iterate

# 评估最近 20 个 sessions
/dev-flow:meta-iterate evaluate --recent 20

# 只关注特定 agent
/dev-flow:meta-iterate --target agents/plan-agent.md

# 应用改进 (需要先 propose)
/dev-flow:meta-iterate apply

# 验证效果 (需要新 sessions)
/dev-flow:meta-iterate verify --iteration ITER-001
```

### 自动提醒

每 10 个 session 后，系统会自动提醒运行 `/dev-flow:meta-iterate`。

## 自动化细节

### /dev-flow:commit

```
1. make fix          # 自动格式化
2. make check        # 验证错误
3. git diff --stat   # 分析变更
4. dev_defaults      # 推断 scope
5. git commit        # 生成 message (无 Claude 署名)
6. dev_reasoning     # 保存决策历史
7. dev_ledger        # 更新状态
```

### /dev-flow:pr

```
1. 检查未提交 → /dev-flow:commit
2. 检查未推送 → git push
3. dev_commits       # 收集提交
4. dev_reasoning     # 聚合 reasoning
5. gh pr create      # 创建 PR (中文描述)
6. code-reviewer     # 自动代码审查
```

### Hooks（自动启用）

插件安装后以下 hooks 自动生效，无需配置：

| Hook | 触发时机 | 功能 |
|------|---------|------|
| **SessionStart** | 恢复 session (resume/compact/clear) | 自动加载 ledger 上下文 |
| **PreCompact** | Context 压缩前 | 备份 transcript 到 `.git/claude/transcripts/` |
| **PostToolUse** | 执行 Bash 命令后 | 提醒使用 `/dev` 命令代替原始 git 操作 |

**位置**: `hooks/hooks.json` (标准路径自动加载)

## MCP 工具

| 工具 | Tokens | 功能 |
|------|--------|------|
| `dev_status` | ~30 | 快速状态 |
| `dev_flow` | ~100 | 详细状态 |
| `dev_check` | ~10 | CI 就绪检查 |
| `dev_next` | ~15 | 下一步建议 |
| `dev_ledger` | ~50 | Ledger 管理 |
| `dev_reasoning` | ~30 | Reasoning 管理 |
| `dev_branch` | ~30 | 分支生命周期 |
| `dev_defaults` | ~20 | 智能默认值 |
| `dev_commits` | ~100 | 提交分组 |
| `dev_version` | ~30 | 版本建议 |

## Agents

### 开发流程 Agents

| Agent | 功能 |
|-------|------|
| plan-agent | 创建实现计划 |
| implement-agent | TDD 执行任务 |
| validate-agent | 验证技术选型 |
| code-reviewer | PR 代码审查 |
| pr-describer | 生成 PR 描述 |
| debug-agent | 调试问题 |

### Meta-Iterate Agents

| Agent | 功能 |
|-------|------|
| evaluate-agent | 分析 session 表现，识别低分组件 |
| diagnose-agent | 诊断组件问题根因 |
| propose-agent | 生成改进提案 (多方案) |
| apply-agent | 应用改进 (带备份) |
| verify-agent | 验证改进效果 |

## 目录结构

```
dev-flow-plugin/
├── .claude-plugin/plugin.json
├── .mcp.json
├── mcp-server/              # MCP 服务器
├── skills/                  # 6 个 skills
│   ├── dev/
│   ├── create_plan/
│   ├── implement_plan/
│   ├── implement_task/
│   ├── validate-agent/
│   └── meta-iterate/
├── commands/                # 15 个命令
├── agents/                  # 12 个 agents
│   ├── plan-agent.md
│   ├── implement-agent.md
│   ├── validate-agent.md
│   ├── code-reviewer.md
│   ├── pr-describer.md
│   ├── debug-agent.md
│   ├── reasoning-generator.md
│   ├── evaluate-agent.md
│   ├── diagnose-agent.md
│   ├── propose-agent.md
│   ├── apply-agent.md
│   └── verify-agent.md
├── hooks/hooks.json
└── templates/
    └── thoughts/schema/
```

## License

MIT
