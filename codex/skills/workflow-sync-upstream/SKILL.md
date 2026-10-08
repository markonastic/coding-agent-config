---
name: workflow-sync-upstream
description: "Pull plugin workflow updates from the original coding-agent-config repo into your fork"
---

In this Codex command adapter, `$ARGUMENTS` means the request supplied with
this skill invocation. If no request was supplied, treat it as empty.

# Sync Upstream

Bring this configuration clone up to date with the original
`alexportelli12/coding-agent-config` repository and mirror the result to the
user's `markonastic/coding-agent-config` fork.

Run:

```bash
scripts/sync-upstream
```

Optional flags (for example `--dry-run`) come from `$ARGUMENTS` when present.

## If It Conflicts

The script leaves the merge in progress when files conflict. Resolve them:

1. Repair conflicts manually, treating upstream as the default source of
   truth for workflow guidance (`AGENTS.md`, `agent/`, `skills/`, `commands/`).
   Resolve review rules such as `scripts/`, `agent/`, and host manifests
   (`opencode.json`, generated agents) toward whatever keeps this
   installation working. Keep any local divergence the user accepted before.
2. Commit the merge, run `scripts/sync-upstream` again, and continue only
   from a green run or an explicit report of what is still failing.
3. `npm run agents:generate` builds the Codex command wrappers from
   `commands/`; `npm run verify` fails when generated output is stale, so
   finish with `agents:generate` before the verify gate.

Report the commit range merged, any conflict decisions, and the verify
result. Stop after reporting; further workflow changes go through
`/update-my-workflow`.
