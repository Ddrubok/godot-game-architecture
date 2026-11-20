# Godot 게임 구조 설계

[English](README.md)

Godot 게임에서 방과 캐릭터를 어떻게 씬으로 나누고 각 상태를 어디에 보관할지 정합니다.

```text
$godot-game-architecture 플레이어가 열쇠를 줍고 방을 나갔다 돌아와. 열쇠를 이미 주웠다는 사실은 어디에 둘까? 방 씬을 단독으로 실행하는 방법과 재진입 검증까지 설계해줘.
```

### G1: 상점과 웨이브 재진입

```text
$godot-game-architecture 테스트 상점에서 10G로 시작해 3G 타워 하나를 산다. 웨이브를 거쳐 상점을 세 번 다시 열 때 잔액·타워 수, 버튼 신호의 수명, 새 런 초기화, 같은 Resource로 만든 타워 두 개의 런타임 값 소유자를 설계해줘.
```

먼저 프로젝트의 메인 씬, 신호 연결, Autoload, 저장 경로를 확인합니다. 한 런의 소유자가 잔액과 보유 타워를 함께 관리하고, 상점 버튼은 구매 요청을 한 번 보냅니다. 소유자는 가격을 확인한 뒤 잔액 7G와 타워 1기를 함께 확정합니다. 버튼 신호는 상점 씬이 활성화될 때 연결하고 그 씬이 끝날 때 함께 해제합니다. 같은 런에서 재진입할 때마다 UI는 소유자에게서 7G·1기를 읽으며, 두 번 더 열어도 구매가 반복되지 않습니다. 새 런은 10G·0기로 시작합니다. 타워 정의 Resource는 공유할 수 있지만 변하는 런타임 값은 각 인스턴스가 소유합니다. 한쪽 값을 바꿔도 다른 타워와 원본 정의는 그대로입니다. 상점 씬 단독 실행에는 이 값을 가진 테스트용 런 상태를 주입할 수 있습니다.

## 설치

[스킬 ZIP 다운로드](https://github.com/Ddrubok/godot-game-architecture/releases/latest/download/godot-game-architecture.zip) 후 Codex 스킬 폴더에 압축을 풉니다. 기본 위치는 `~/.codex/skills`이고, `CODEX_HOME`을 설정했다면 `$CODEX_HOME/skills`입니다. 압축 파일 안에 이미 `godot-game-architecture` 폴더가 들어 있습니다. 설치 후 `godot-game-architecture/SKILL.md`에서 지침을 확인할 수 있습니다.

이 스킬은 단독으로 쓸 수 있습니다. 범용 스킬을 사용 중이라면 설계 도중 필요한 시점에 이 스킬을 받습니다. [범용 소프트웨어 구조 설계](https://github.com/Ddrubok/software-architecture)

범용 스킬을 설치하지 않아도 직접 호출할 수 있습니다. `VERSION`은 선언된 태그입니다. 배포 파일을 검증하지 않았다면 “선언 v1.1.0 (미검증)”으로만 보고하고 검증된 배포 버전이라고 말하지 않습니다.
