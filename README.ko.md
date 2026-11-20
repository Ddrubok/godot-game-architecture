# Godot 게임 구조 설계

[English](README.md)

Godot 게임에서 방과 캐릭터를 어떻게 씬으로 나누고 각 상태를 어디에 보관할지 정합니다.

```text
$godot-game-architecture 플레이어가 열쇠를 줍고 방을 나갔다 돌아와. 열쇠를 이미 주웠다는 사실은 어디에 둘까? 방 씬을 단독으로 실행하는 방법과 재진입 검증까지 설계해줘.
```

## 설치

[스킬 ZIP 다운로드](https://github.com/Ddrubok/godot-game-architecture/releases/latest/download/godot-game-architecture.zip) 후 Codex 스킬 폴더에 압축을 풉니다. 기본 위치는 `~/.codex/skills`이고, `CODEX_HOME`을 설정했다면 `$CODEX_HOME/skills`입니다. 압축 파일 안에 이미 `godot-game-architecture` 폴더가 들어 있습니다. 설치 후 `godot-game-architecture/SKILL.md`에서 지침을 확인할 수 있습니다.

이 스킬은 단독으로 쓸 수 있습니다. 범용 스킬을 사용 중이라면 설계 도중 필요한 시점에 이 스킬을 받습니다. [범용 소프트웨어 구조 설계](https://github.com/Ddrubok/software-architecture)
