## Summary

-

## Verification

- [ ] `cargo fmt --check`
- [ ] `cargo test -p vibemud-db`
- [ ] `cargo test -p vibemud-cli`
- [ ] Python/Bash/npm checks when relevant

## Terminal coverage

List tested environments or mark not applicable:

- [ ] cmux
- [ ] tmux
- [ ] Ghostty
- [ ] iTerm2
- [ ] VS Code integrated terminal
- [ ] Windows PowerShell
- [ ] Windows Terminal
- [ ] Plain terminal fallback

## Release/privacy checklist

- [ ] No source files, prompts, transcripts, editor buffers, or agent conversation content are used as game state.
- [ ] Windows-native behavior does not require WSL, Bash, or tmux.
- [ ] Codex and iTerm2 remain marked unsupported unless new smoke evidence and maintainer approval are included.
- [ ] New terminal support claims are backed by smoke evidence or labeled as pending/fallback.
