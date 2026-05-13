# Claude Code instructions for VibeMUD

VibeMUD is a local Rust CLI idle RPG for Claude Code. It should feel like a sidecar game beside the coding session, not part of the coding context.

## Current public support boundary

- Supported user path: Claude Code plugin with `/vibemud:mud ...` commands.
- Verified terminal panes: tmux, cmux, macOS Ghostty.
- Unsupported: Codex, Codex `$mud`, Codex hook/session setup, and iTerm2 native automation.
- README must keep the user guide centered on this Claude Code command set:

```text
/vibemud:mud start
/vibemud:mud c
/vibemud:mud i
/vibemud:mud m
/vibemud:mud q
/vibemud:mud set
/vibemud:mud end
```

## Privacy and context rules

- Never use project source, prompts, transcripts, editor buffers, or Claude conversation content as game state.
- Keep game state in the VibeMUD data directory and SQLite DB only.
- Plugin hooks should intercept VibeMUD commands before they enter Claude's main prompt and return short safe acknowledgements by default.
- Game logs, HUD frames, rewards, combat text, and inventory details should not be dumped into the coding conversation unless the user explicitly asks for verbose/live output.

## UX rules

- Keep the coding pane usable. Prefer side panes/splits for HUD and selectors.
- Pane preference: cmux, then tmux, then macOS Ghostty, then safe fallback.
- `start` and `panel` should open or reuse a right-side HUD pane.
- Selector commands (`m`, `i`, `q`, `set`) may focus an auxiliary pane/split when supported, then return to the normal HUD on close.
- `end`/`stop` should stop hunt/runtime and close recorded HUD panes where possible.

## Repository rules

- This public repository intentionally excludes `docs/` and local agent/runtime folders. Do not add them back unless the maintainer changes the release policy.
- Root `AGENTS.md` and `CLAUDE.md` are the only agent guidance files intended for GitHub.
- Keep setup paths simple: npm install for users, `scripts/install.sh --for claude --scope local` for local Claude Code development.
- Do not re-add Codex install instructions or Codex quick starts while Codex remains unsupported.

## Verification

Before claiming completion, run the targeted checks in `AGENTS.md` or explain why a platform-specific smoke test could not be run.
