# Overstory — V1 Scope

## One-Liner
Multi-agent orchestration system that spawns coordinated Claude Code (or alternative runtime) agents in git worktrees, coordinating them through SQLite mail and merging their work with tiered conflict resolution.

## V1 Definition of Done

- [ ] Agent spawning works: `sling` creates worktree + tmux session + injects CLAUDE.md overlay for any agent role
- [ ] Agent lifecycle works: `stop` terminates agents cleanly, `status` shows fleet state
- [ ] Coordinator pattern works: `coordinator start/stop/status/send/ask` manages persistent orchestrator
- [ ] Mail system works: `mail send/check/list/read/reply/purge` enables inter-agent communication
- [ ] Merge system works: `merge` resolves worktree branches with Tier 1 (trivial) and Tier 2 (mechanical) resolution
- [ ] Two runtime adapters stable: Claude Code (for coordinators/leads) and Sapling (for leaf agents: builders, scouts, reviewers)
- [ ] `ov sling --runtime sapling` successfully dispatches leaf agents that complete tasks and exit cleanly
- [ ] Sapling runtime adapter correctly passes guards file, worktree path, and file scope to `sp run`
- [ ] Task tracking integration works: reads issues from seeds (`sd ready --json`, `sd update`, `sd close`)
- [ ] Observability basics work: `status`, `inspect`, `logs`, `errors`
- [ ] `doctor` health checks pass on a correctly configured system
- [ ] Worktree management works: `worktree list/clean`
- [ ] Agent definitions (base .md files) exist for core roles: scout, builder, reviewer, merger, lead, coordinator
- [ ] Run lifecycle works: `run list/show/complete`
- [ ] Hooks install/uninstall correctly configures Claude Code settings
- [ ] `prime` outputs usable orchestrator context
- [ ] All tests pass (`bun test`)
- [ ] TypeScript strict mode clean (`bun run typecheck`)
- [ ] Linting passes (`bun run lint`)
- [ ] CI pipeline runs lint + typecheck + test on push/PR
- [ ] Published to npm as `@os-eco/overstory-cli`

## Explicitly Out of Scope for V1

- Merger agent spawning for Tier 3+ conflicts (open issue `overstory-inxu`) — 1-shot resolver is sufficient for V1
- SessionInsight auto-recording via mulch (open issue `overstory-vo3e`) — manual `ml record` is fine for V1
- Native canopy integration with render-on-demand (open issue `overstory-bdbe`) — static .md files work for V1
- Dashboard TUI polish beyond basic `ov dashboard`
- Cost budgeting or spend limits (cost tracking exists, enforcement does not)
- Multi-repo orchestration (coordinating agents across different repositories)
- Persistent agent state across sessions (checkpoint/resume is partial)
- Alternative runtime stability beyond Claude Code and Sapling (pi, copilot, codex, gemini, opencode are experimental)
- Automated retry/recovery of failed agents without human intervention
- Web UI for monitoring or controlling agents
- Replay command for post-mortem analysis (exists but not critical path)

## Current State

Overstory is the most complex tool in the ecosystem and is near V1-complete. All 34 CLI commands are implemented and tested. 3,283 tests pass. TypeScript and linting are clean. CI is green. Published to npm at v0.8.6.

The core workflow — spawn agents in worktrees, coordinate via mail, merge results — is functional and battle-tested through real swarm runs. The Claude Code runtime adapter is stable. The sapling runtime adapter exists but needs end-to-end validation with real swarm runs (leaf agents completing tasks). The coordinator pattern has replaced the deprecated supervisor pattern.

The 3 open issues are all Priority 3 features (merger agent triggering, session insight auto-recording, canopy integration) — none block V1 functionality.

**Estimated completion: ~90%.** The remaining 10% is stability hardening of the merge pipeline and edge cases in multi-agent coordination that only surface under real workloads.

## Open Questions

- Is the deprecated supervisor agent code (still present) blocking V1, or can it be removed in a cleanup pass?
- Should the 5 experimental runtime adapters (pi, copilot, codex, gemini, opencode) be formally marked as "alpha" or removed from V1? Sapling is now required as the leaf agent runtime.
- What's the minimum acceptable merge resolution? Is Tier 1 + Tier 2 sufficient, or does V1 require Tier 3 (merger agent) for real-world multi-agent work?
- The watchdog system has 3 tiers — which tiers need to be stable for V1? (Tier 0 mechanical is working; Tier 1 AI-assisted and Tier 2 monitor are less proven.)
