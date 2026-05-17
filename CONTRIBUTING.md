# Contributing to VibeMUD

Thanks for helping make VibeMUD a better local sidecar RPG for developer terminals.

## Before you start

- Keep the privacy boundary intact: VibeMUD must not use source files, prompts, transcripts, editor buffers, or agent conversation content as game state.
- Keep game mutations runtime-owned through the command queue unless a command is explicitly read-only.
- Keep terminal UX non-blocking where possible; use side panes/splits when supported and documented fallbacks otherwise.
- Do not add `~mud` command routing. Supported Claude commands are `/mud` and `/vibemud:mud`.
- Native Windows support must not depend on WSL, Bash, or tmux.


## Current public support boundary

- User-facing quick start is Claude Code only: `/vibemud:mud ...`.
- Verified pane targets are tmux, cmux, and macOS Ghostty.
- Codex and iTerm2 are unsupported until fresh smoke evidence and maintainer approval change the public claim.
- npm `latest` is `vibemud@0.1.27` as of 2026-05-18; this is the fixed multi-platform release.

## Local checks

Run the smallest relevant checks for your change. For CLI/plugin changes, start with:

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

## Terminal smoke checks

Before changing terminal/session behavior, update `README.md` when public support changes and record smoke evidence in the PR or a public `terminal_smoke` issue.

Core-six smoke flow:

1. install or local package install
2. `vibemud init`
3. `vibemud statusline`
4. `mudctl hunt start --area forest-edge --auto-start`
5. HUD/side pane or documented fallback opens
6. `vibemud session stop` / cleanup works

## Pull request expectations

- Describe the user-facing behavior change.
- List terminal environments tested or explicitly not tested.
- Include verification commands and results.
- Call out privacy, Windows-native, and packaging implications when relevant.
