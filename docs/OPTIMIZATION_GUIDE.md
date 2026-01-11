# Claude Code 配置优化指南

持续收集的配置优化经验和最佳实践。

## 信息来源

### 官方渠道

| 来源 | URL | 更新频率 |
|------|-----|---------|
| **Release Notes** | https://github.com/anthropics/claude-code/releases | 每次发布 |
| **官方博客** | https://claude.ai/blog | 不定期 |
| **官方文档** | https://docs.anthropic.com/en/docs/claude-code | 持续更新 |

### 监控方式

```bash
# GitHub Release 通知
# 1. Star + Watch 仓库: https://github.com/anthropics/claude-code
# 2. 设置 Watch → Custom → Releases

# RSS Feed (可选)
# https://github.com/anthropics/claude-code/releases.atom
```

---

## Skill 开发最佳实践

### Frontmatter 必需字段

```yaml
---
name: skill-name              # lowercase, hyphens
description: [What]. Use when user says "[triggers]", "[中文触发词]".
allowed-tools: [Read, Bash]   # 限制工具访问
---
```

### 可选字段

| 字段 | 用途 | 示例 |
|------|------|------|
| `model` | 指定模型 | `opus`, `haiku` |
| `context: fork` | 隔离 context | 复杂 agent |
| `user-invocable: false` | 隐藏 slash 菜单 | 内部 helper |
| `disable-model-invocation: true` | 禁止自动触发 | 危险操作 |

### Progressive Loading 架构

```
skill/
├── SKILL.md              # < 150 行，精简概览
└── references/
    ├── templates.md      # 完整模板
    ├── workflow.md       # 详细流程
    └── examples.md       # 使用示例
```

**原则**: SKILL.md 只包含概览 + Reference Menu，详细内容按需加载。

### Description 编写要点

1. 第一句说明功能
2. 列出触发场景
3. 包含中英文触发词

```yaml
# ✅ Good
description: Validate plan tech choices against best practices. Use when user says "validate plan", "check tech", "验证方案".

# ❌ Bad
description: Helps with validation  # 太模糊
```

---

## Hook 开发最佳实践

### 8 种 Hook 类型

| Hook | 触发 | 可阻止 | 主要用途 |
|------|-----|--------|---------|
| PreToolUse | 工具执行前 | ✅ | 阻止/修改工具调用 |
| PostToolUse | 工具执行后 | 部分 | 格式化/lint |
| UserPromptSubmit | 用户发送 | ✅ | 注入上下文 |
| PermissionRequest | 权限对话框 | ✅ | 自动批准/拒绝 |
| SessionStart | 会话开始 | ❌ | 加载上下文 |
| Stop | Claude 完成 | ✅ | 强制继续 |
| SubagentStop | 子 agent 完成 | ✅ | 验证输出 |
| PreCompact | 压缩前 | ❌ | 保存状态 |

### SessionStart 优化 (v2.1.2+)

```bash
# 检查 agent_type，跳过非主 agent
AGENT_TYPE=$(echo "$INPUT" | jq -r '.agent_type // "main"')
if [[ "$AGENT_TYPE" != "main" ]]; then
    echo '{"result": "continue"}'
    exit 0
fi
```

### Matcher 语法

```json
{
  "matcher": "Write|Edit",           // 多个工具
  "matcher": "*",                    // 所有工具
  "matcher": "Bash(npm test*)",      // 带参数
  "matcher": "mcp__memory__.*"       // MCP 工具
}
```

---

## 版本更新记录

### v2.1.3 (2026-01)

| 特性 | 配置变更 |
|------|---------|
| Skills/Commands 合并 | 删除重复 command 文件 |
| Hook timeout 10min | 增加长 hook 超时 |
| 死规则检测 | `/doctor` 检查 |

### v2.1.2 (2026-01)

| 特性 | 配置变更 |
|------|---------|
| `agent_type` in SessionStart | 跳过子 agent 重处理 |
| `FORCE_AUTOUPDATE_PLUGINS` | settings.json env |

---

## Subagent 最佳实践

### Task Tool 参数

```python
Task(
  description="3-5 字描述",     # 必需
  prompt="详细任务说明",        # 必需
  subagent_type="general-purpose",  # 必需
  model="sonnet",              # 可选: opus, sonnet, haiku
  run_in_background=False,     # 可选: 后台运行
  resume="<agentId>"           # 可选: 恢复之前的代理
)
```

### 模型选择策略

| 任务类型 | 推荐模型 | 原因 |
|---------|---------|------|
| 简单搜索/lint | `haiku` | 快速、低成本 |
| 标准实现 | `sonnet` | 平衡 (默认) |
| 复杂分析/安全 | `opus` | 最强能力 |

### 工具限制模式

| 场景 | 推荐工具 |
|------|---------|
| 只读分析 | `[Read, Grep, Glob]` |
| 测试执行 | `[Bash, Read, Grep]` |
| 代码修改 | `[Read, Edit, Write, Bash]` |
| 研究收集 | `[WebSearch, WebFetch, Read]` |

### Context Fork

```yaml
# SKILL.md frontmatter
---
context: fork  # 隔离 context，防止污染主代理
---
```

**优点**:
- 子代理 context 不污染主代理
- 节省主代理 context 窗口
- 更清晰的任务边界

### Resume 能力

```python
# 恢复之前的代理，保留完整 context
Task(
  resume="<agentId>",
  prompt="继续之前的分析..."
)
```

### 并行执行

```python
# 同一消息中生成多个 Task（无依赖时）
Task(description="Task 1", ...)
Task(description="Task 2", ...)
```

### 限制

- ❌ 子代理不能嵌套（不能再生成子代理）
- ❌ Windows 长 prompt 限制 8191 字符

详见: `docs/SUBAGENT_PATTERNS.md`

---

## CLAUDE.md 最佳实践

### 推荐章节

| 章节 | 内容 |
|------|------|
| Quick Reference | 常用命令表格 |
| Directory Structure | 带注释的目录树 |
| Standards | 代码规范 |
| Skills | 可用 skill 列表 |

### 反模式

- ❌ 超过 500 行
- ❌ 重复代码中的信息
- ❌ 包含 API key/secrets
- ❌ 过时文档

---

## 优化检查清单

### Skill 检查

- [ ] `name` 字段存在
- [ ] `description` 包含触发词 (EN + CN)
- [ ] `allowed-tools` 限制不必要工具
- [ ] SKILL.md < 150 行
- [ ] 复杂内容移到 references/

### Hook 检查

- [ ] SessionStart 检查 `agent_type`
- [ ] 使用 `$CLAUDE_PROJECT_DIR` 路径
- [ ] 错误处理完善
- [ ] 超时设置合理

### 规则检查

- [ ] 运行 `/doctor` 检查死规则
- [ ] 规则文件 < 200 行
- [ ] 无冲突规则

---

## 自动化工具

### /config-optimize

定期运行检查新特性:

```bash
/config-optimize          # 完整优化流程
/config-optimize check    # 仅检查
```

### /meta-iterate

分析会话性能优化提示:

```bash
/meta-iterate            # 完整 5 阶段流程
```

---

## 贡献指南

发现新的优化模式时:

1. 更新 `docs/OPTIMIZATION_GUIDE.md`
2. 更新 `skills/config-optimize/references/version-history.md`
3. 如果是重要模式，考虑创建新 rule

## 参考链接

- [Claude Code Releases](https://github.com/anthropics/claude-code/releases)
- [Skills Explained](https://claude.ai/blog/skills-explained)
- [How to Configure Hooks](https://claude.ai/blog/how-to-configure-hooks)
- [Using CLAUDE.md Files](https://claude.ai/blog/using-claude-md-files)
