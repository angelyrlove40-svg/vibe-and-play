# Security Policy

## Supported versions

VibeMUD is pre-1.0. Security fixes target the latest `main` branch and the latest published release once public releases begin.

## Reporting a vulnerability

Please do not open public issues for vulnerabilities or leaked secrets. Report privately by contacting the repository owner through GitHub, or use GitHub private vulnerability reporting if enabled for the public repository.

Include:

- affected commit/version
- reproduction steps
- impact
- whether credentials, local files, prompts, transcripts, or editor buffers can be exposed

## Security boundaries

VibeMUD is a local game/runtime. It must not read source files, prompts, transcripts, editor buffers, or agent conversation content as game state. Claude Code integration must keep game commands and game output separate from the coding conversation unless the user explicitly opts into verbose/live output. Codex is currently unsupported and must not be presented as a supported integration.

## Before public release

Run secret scans and terminal smoke checks before public release. Record release evidence in the PR or a public maintainer issue, not in a committed `docs/` folder.
