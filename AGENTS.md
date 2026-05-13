# Agent instructions for VibeMUD

## Scope

These instructions apply to the whole repository.

VibeMUD is a Rust-based local idle RPG that runs beside Claude Code through a Claude Code plugin. The public repository is intentionally minimal: source code, npm packaging, Claude plugin files, GitHub automation, OSS policy files, README, and this agent guidance only.

## Product contract

- Primary supported integration: Claude Code slash commands through `/vibemud:mud ...`.
- Verified pane targets: tmux, cmux, and macOS Ghostty.
- Unsupported targets: Codex and iTerm2. Do not reintroduce Codex `$mud`, Codex hook installers, or iTerm2 support claims without fresh smoke evidence and an explicit maintainer decision.
- Preserve the privacy boundary: never use source files, prompts, transcripts, editor buffers, or agent conversation content as game state.
- Game state belongs only in the configured VibeMUD data directory and SQLite DB, not in project files.
- Game mutations should go through the runtime command queue unless a command is documented as read-only or immediate.
- Do not add `~mud` routing. Supported Claude command surfaces are `/mud` and `/vibemud:mud` only.

## Repository hygiene

- Keep the public GitHub tree minimal. Do not add `docs/`, PRD drafts, screenshots, local agent folders, runtime DBs, logs, generated package artifacts, or local terminal helper scripts to Git.
- Root `AGENTS.md` and `CLAUDE.md` are intentional public guidance files and should stay at repository root.
- Prefer updating `README.md`, `CONTRIBUTING.md`, or `SECURITY.md` over adding new public docs.
- Avoid new dependencies unless necessary for the shipped CLI/plugin and documented in the relevant manifest.

## Development rules

- Prefer Rust core changes for behavior that must work across platforms.
- Keep shell/PowerShell/plugin changes small and syntax-checkable.
- Keep coding/game UI separated; side panes should not occupy the main Claude Code pane when a supported pane target is available.
- If a platform cannot be smoke-tested locally, state that gap explicitly instead of claiming support.

## Verification

For CLI/plugin changes, run the smallest relevant checks and report results:

```bash
cargo fmt --check
cargo test -p vibemud-db
cargo test -p vibemud-cli
python3 -m py_compile claude-marketplace/plugins/vibemud/scripts/vibemud-context-hook.py
bash -n scripts/install.sh scripts/install-claude-plugin.sh claude-marketplace/plugins/vibemud/scripts/vibemud-claude.sh
```

For npm/package changes, also run:

```bash
node npm/scripts/check-release-metadata.js
(cd npm && npm run test:resolve)
(cd npm && npm pack --dry-run --json | node scripts/check-pack-contents.js)
```
