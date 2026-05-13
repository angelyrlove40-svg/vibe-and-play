---
description: "VibeMUD 조작: c 캐릭터, i 장비, m 지도, q 퀘스트, start 시작, end 종료"
argument-hint: "c|i|m|q|set|start|end|help"
allowed-tools: ["Bash"]
hide-from-slash-command-tool: "false"
---

# VibeMUD Command

VibeMUD commands are normally intercepted by the plugin `UserPromptExpansion`/`UserPromptSubmit` context firewall before this prompt reaches Claude. The hook executes the command, updates the local DB/HUD pane, then blocks the prompt so game commands and game output do not enter the main coding context.

Slash-command preview contract:

- `description` must stay operation-focused and short: `c`, `i`, `m`, `q`, `set`, `start`, `end`, `help`.
- `argument-hint` must list only high-value entry points, not every low-level `mudctl` subcommand.
- The full guide lives in `scripts/vibemud-claude.sh help`; do not duplicate the whole guide here.
- Keep Korean and English aliases consistent with the dispatcher:
  - `help|도움말|가이드`
  - `c|character|캐릭터`
  - `i|inventory|item|items|장비|소지품|가방`
  - `m|map|menu|지도|메뉴|던전|지역`
  - `q|quest|quests|퀘스트|일일퀘스트`
  - `set|settings|setting|설정|환경설정`
  - `start|시작|플레이|play`
  - `end|stop|정지|종료|pause|중지`

If hooks are unavailable, run the dispatcher in safe mode with the fixed help command only. This fallback must not pass raw slash arguments through a shell; the normal hook path owns command execution.

```!
VIBEMUD_CONTEXT_MODE=safe bash "${CLAUDE_PLUGIN_ROOT}/scripts/vibemud-claude.sh" help
```

Do not summarize, interpret, or restate game state from VibeMUD command output. Tell the user to look at the HUD pane for game details. Shortcuts: `start` starts or resumes the game, `end` stops it, `c` opens character details, `i` opens the equipment/inventory manager, `m` opens the dungeon/area map selector, `q` opens daily quests, `set` opens settings actions, and `help` prints the one-page operation guide. Auxiliary screens should be closed from inside the screen with `q`. The `c`, `m`, `i`, `q`, and `set` selectors open an interactive selector pane and focus it when `ui.popup_pane_enabled=true`; closing the selector returns that auxiliary pane to the normal HUD instead of replacing the user's current pane. Otherwise they leave the relevant HUD/output visible so the user can type a direct command. Do not read source files, prompts, transcripts, or IDE/editor buffers for game context; VibeMUD state lives in its own local game database.
