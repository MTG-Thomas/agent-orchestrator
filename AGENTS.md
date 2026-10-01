# Agent Orchestrator guidance

This MTG fork manages coding sessions through runtime, agent, workspace, tracker, and SCM plugins. Preserve upstream extension interfaces and cross-platform behavior. Load only the relevant reference sections: [session architecture](CLAUDE-DETAILS.md#session-lifecycle) for lifecycle work; [CLI semantics](CLAUDE-DETAILS.md#cli-behavior-ao-start--ao-stop) for start/stop/restore; [plugin contracts](CLAUDE-DETAILS.md#plugin-standards) for extensions; [code conventions](CLAUDE-DETAILS.md#conventions) for implementation; [bug triage](AGENT-DETAILS.md#bug-triage-skillsbug-triage) for reported defects. The references retain upstream detail; no blanket full-file read is required.

## Map and invariants

`packages/core/` owns types, configuration, sessions, lifecycle, and platform helpers; `packages/cli/` owns `ao`; `packages/web/` owns the dashboard; `packages/plugins/` supplies extension slots. Read `types.ts` before interface changes. For lifecycle/session changes, state which invariants are preserved and cover canonical-to-legacy status derivation, stale-runtime reconciliation, and restore behavior.

Use `isWindows()` from `@aoagents/ao-core`, never a new inline `process.platform === "win32"` check. Extend `platform.ts` for missing helpers. Read [docs/CROSS_PLATFORM.md](docs/CROSS_PLATFORM.md) before changing process control, paths, shell commands, network binding, agent/runtime/workspace plugins, or Windows PTY behavior. Windows, Linux, and macOS are first-class.

## Verified command surface

Use the root's pinned pnpm and Node versions. `pnpm install`, `pnpm build`, `pnpm typecheck`, `pnpm test`, `pnpm lint`, and `pnpm format:check` are root scripts. Root tests exclude web: run `pnpm --filter @aoagents/ao-web test` for web changes. Integration work uses `pnpm test:integration` and its documented prerequisites.

For reported bugs, read `skills/bug-triage/SKILL.md`; fetch and inspect current main without discarding dirty work or changing an active checkout. Keep fixes surgical, define verifiable success, and reproduce defects at the affected seam. Read release/Changesets workflows before publishing.

Live session launch, cross-project stop/restore, notifications, and PR/API mutation are operational actions requiring the authorized target and scope. Do not run them merely to validate documentation.

## Universal constraints

Keep pnpm `workspace:*` cross-package dependencies and conventional commits; use Changesets for versioning and retain gitleaks hooks. Preserve the 5-second SSE interval. Web changes keep the dark theme and Next.js App Router, add tests for new components, keep component files at most 400 lines, and add no UI component libraries, animation libraries, or inline styles.
