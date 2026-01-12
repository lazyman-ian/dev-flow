# Changelog

All notable changes to dev-flow-plugin will be documented in this file.

Author: lazyman

## [3.6.2] - 2026-01-11

### Changed
- Bundle MCP server to single `scripts/mcp-server.cjs` file using esbuild
- No more TypeScript compilation required on plugin install
- Update `.mcp.json` to use bundled file

### Fixed
- Plugin installation failure when `mcp-server/dist/` not compiled

## [3.6.1] - 2026-01-11

### Added
- Complete hooks to plugin: dev-workflow.sh, session-start-continuity.sh, pre-compact.sh
- Self-contained hook configuration in hooks.json

## [3.6.0] - 2026-01-11

### Added
- Subagent optimization patterns from official documentation
- `context: fork` to meta-iterate, validate-agent skills
- `model` selection to dev skill (sonnet)
- `user-invocable: false` to internal helper skills
- Reference File Architecture for all major skills
- `docs/OPTIMIZATION_GUIDE.md` - comprehensive optimization patterns
- `docs/SUBAGENT_PATTERNS.md` - subagent best practices

### Changed
- Optimized skill SKILL.md files (60-80% line reduction)
- Moved detailed content to `references/` directories
- Updated frontmatter with official best practices

## [3.5.0] - 2026-01-10

### Added
- `/config-optimize` skill for tracking Claude Code updates
- Reference File Architecture pattern
- Progressive loading for skills

### Changed
- Restructured major skills with references/ directories
- create_plan: 486 → 97 lines
- implement_plan: 316 → 106 lines
- implement_task: 297 → 84 lines
- validate-agent: 254 → 100 lines

## [3.1.0] - 2026-01-08

### Added
- `/meta-iterate` skill for self-improvement workflow
- 5-phase iteration: evaluate → diagnose → propose → apply → verify
- Meta-iterate agents: evaluate, diagnose, propose, apply, verify
- `thoughts/schema/` templates for structured outputs
- Session analysis integration with Braintrust

### Changed
- Enhanced agent orchestration patterns
- Improved handoff chain mechanism

## [3.0.0] - 2026-01-05

### Added
- Agent orchestration for complex implementations
- Continuity ledgers for task tracking
- Reasoning documentation for commits
- TDD workflow integration

### Changed
- Unified `/dev-flow:` command interface
- Platform-agnostic design (iOS, Android, Web)

## [2.0.0] - 2025-12-20

### Added
- MCP server integration (dev-flow-mcp)
- Token-optimized tools (dev_status, dev_flow, etc.)
- Automated scope inference
- PR description generation

## [1.0.0] - 2025-12-01

### Added
- Initial release
- Basic `/dev-flow:commit`, `/dev-flow:pr`, `/dev-flow:release` commands
- Git workflow automation
