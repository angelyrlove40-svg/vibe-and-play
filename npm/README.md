# VibeMUD npm package

VibeMUD는 개발자 터미널 옆에서 돌아가는 **로컬 방치형 MUD/RPG**입니다. npm package는 Rust로 빌드된 네 개의 CLI를 Node shim으로 실행합니다.

- `vibemud`
- `mudctl`
- `vibemud-runtime`
- `vibemud-hud`

## 설치

2026-05-16 확인 기준 npm `latest`는 **`vibemud@0.1.25` macOS Apple Silicon(darwin/arm64) host-only preview**입니다. 이 package source는 다음 배포 후보(`0.1.26`)이며, multi-platform `latest`는 matching `@vibemud/native-*` optional package가 먼저 publish된 뒤에만 배포합니다.

Repository metadata는 `https://github.com/angelyrlove40-svg/vibe-and-play`를 기준으로 유지합니다.

```bash
npm install -g vibemud
vibemud --help
mudctl --help
```

일회성 실행:

```bash
npx vibemud@latest --help
npm exec --package vibemud@latest -- vibemud --help
```

`npx`/`npm exec` 자체는 package를 npm cache/임시 실행 경로에서 실행합니다. VibeMUD 게임 데이터는 별도 경로에 저장됩니다.

- 기본 user 저장소: macOS/Linux `~/.vibemud`, Windows `%LOCALAPPDATA%\VibeMUD`
- project 저장소: 프로젝트 루트에서 `vibemud setup --storage project` 실행 시 `.vibemud/` 사용
- 일회성 override: `VIBEMUD_HOME="$PWD/.vibemud" npx vibemud@latest init`


간단 실행:

```bash
vibemud init
mudctl hunt start --area forest-edge --auto-start
vibemud statusline
vibemud session stop
```

## 현재 배포 범위

첫 수동 publish는 `node npm/scripts/stage-packages.js --stage stage1 --host-only`로 만든 host-only monolithic package였습니다. 정식 multi-platform package는 root package가 matching native optional package나 로컬 Cargo source fallback을 찾지 못하면 설치 단계에서 실패해야 하며, native binary를 찾지 못하는 깨진 CLI를 설치하지 않는 것이 목표입니다.

정식 multi-platform `latest`는 다음 순서로 배포합니다.

1. CI에서 각 플랫폼 Rust binary 생성
2. `@vibemud/native-*` optional package들을 먼저 publish
3. 마지막에 root `vibemud` package를 넓은 OS/CPU 대상으로 publish

## Native binary 해석 순서

1. 명시적 환경 변수: `VIBEMUD_BIN`, `MUDCTL_BIN`, `VIBEMUD_RUNTIME_BIN`, `VIBEMUD_HUD_BIN`
2. 현재 플랫폼에 맞는 `@vibemud/native-*` optional package
3. package 안의 `native/<platform>-<arch>/` bundled binary
4. legacy `native/<binary>` bundled binary
5. 로컬 개발용 Cargo target: `target/release`, `target/debug`

Linux npm binary는 첫 정식 배포에서 glibc를 우선합니다. musl/Alpine은 후속 hardening lane입니다.

## 개인정보/코딩 컨텍스트 경계

VibeMUD는 게임 상태를 전용 로컬 데이터 디렉터리에 저장합니다. 소스 파일, 프롬프트, transcript, 에디터 버퍼, agent 대화 내용은 게임 상태로 사용하지 않습니다.
