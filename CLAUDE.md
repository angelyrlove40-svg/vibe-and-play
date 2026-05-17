# Claude Code instructions for VibeMUD

VibeMUD is a Rust-based local idle MUD/RPG that runs beside Claude Code as a sidecar game. Keep coding context and game state separate.

## Current support boundary

- Primary supported integration: Claude Code plugin slash commands: `/vibemud:mud ...` and `/mud ...`.
- Verified pane targets: tmux, cmux, and macOS Ghostty.
- Unsupported unless fresh smoke evidence and maintainer approval exist: Codex `$mud`, Codex hook installers/session setup, iTerm2 native automation, and `~mud` routing.
- Source/package version in this repo is `0.1.27`; npm `latest` checked on 2026-05-18 is `vibemud@0.1.27`.

Keep the public quick start centered on:

```text
/vibemud:mud start
/vibemud:mud c
/vibemud:mud i
/vibemud:mud m
/vibemud:mud q
/vibemud:mud set
/vibemud:mud end
```

## npm release status

- Public install path: `npm install -g vibemud` or explicit `vibemud@latest`; do not recommend pinning `0.1.26`.
- `0.1.27` is the current fixed multi-platform npm release with all six `@vibemud/native-*` packages published.
- `0.1.26` was published but had a native executable permission issue on macOS installs (`EACCES` from 0644 binaries); treat it as superseded.
- Preserve native/root postinstall executable-bit repair and tests unless a registry install smoke proves they are no longer needed.

## Product and privacy contract

- Never use source files, prompts, transcripts, editor buffers, commit text, or agent conversation content as game state.
- Store game state only in the configured VibeMUD data directory and SQLite DB.
- Game mutations should go through the runtime command queue unless a command is documented as read-only or immediate.
- Plugin hooks should intercept VibeMUD slash commands before they enter Claude's main prompt.
- Default command responses should be short acknowledgements; do not dump HUD frames, combat logs, rewards, inventory, or quest details into the coding conversation unless the user explicitly asks for verbose/live output.

## User experience rules

- Keep the main Claude Code pane usable; prefer side panes/splits for HUDs and selectors.
- Pane preference: cmux, then tmux, then macOS Ghostty, then safe/plain fallback.
- `start` and `panel` should open or reuse a right-side HUD pane where supported.
- Selector commands (`c`, `i`, `m`, `q`, `set`) may focus an auxiliary pane/split, then return to the normal HUD on close.
- `end`/`stop` should stop the runtime and close recorded HUD panes where possible.

## Repository surface map

Public tracked source should stay minimal:

- Root policy/docs: `README.md`, `CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`, `LICENSE`, `AGENTS.md`, `CLAUDE.md`.
- Rust workspace: `Cargo.toml`, `Cargo.lock`, `crates/vibemud-*`.
- Claude plugin/marketplace source: `.claude-plugin/`, `claude-marketplace/`.
- npm package source: `npm/`.
- Release/OSS automation: `.github/`.
- Supported installers only: `scripts/install.sh`, `scripts/install.ps1`, `scripts/install-claude-plugin.sh`, `scripts/uninstall-claude-plugin.sh`.
- README demo assets: `assets/`.

Local-only or generated folders/files are intentionally ignored and should not be added to Git:

- Agent/orchestration state: `.omx/`, `.omc/`, `.claude/`, `.agents/`, `.codex/`, `_bmad/`, `codex/`, `tasks/`, `skills-lock.json`.
- Build/package output: `target/`, `dist/`, `artifacts/`, `coverage/`, `*.tgz`.
- Runtime data: `vibemud.db*`, `config.toml`, `panel-pane`, `logs/`, `backups/`.
- Private planning/reference docs: `docs/`.
- Preview/source media not used by README: `preview-assets/`.
- Local unsupported helper scripts such as Codex hook prototypes must remain untracked unless the support boundary changes.

If public guidance is needed, update `README.md`, `CONTRIBUTING.md`, `SECURITY.md`, `AGENTS.md`, or this file instead of adding new public docs.

## Development rules

- Prefer Rust core changes for behavior that must work across platforms.
- Keep shell, PowerShell, npm, and plugin changes small and syntax-checkable.
- Avoid new dependencies unless necessary for the shipped CLI/plugin and reflected in the relevant manifest.
- Do not reintroduce Codex or iTerm2 support claims without new smoke evidence and explicit maintainer approval.
- If a platform cannot be smoke-tested locally, state that gap instead of claiming support.

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
