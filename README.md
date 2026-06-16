# Necrocis Gameplay

Unity 기반 2.5D 로그라이크 액션 게임 프로토타입입니다. 이 저장소는 이력서/포트폴리오 제출용으로 게임플레이 시스템, 기술스택, 코드 구조를 확인하기 쉽도록 정리한 버전입니다.

## 개발 상태

- 플레이어 전투, 성장, 적 AI, 보스 패턴, 바이옴 이동, UI 등 핵심 게임 시스템을 구현했습니다.
- 맵과 바이옴 레이아웃은 아직 미완성 상태이며 계속 제작 중입니다.
- 현재 포함된 맵, 보스방, 바이옴 배치는 최종 콘텐츠가 아니라 개발 중인 작업물입니다.
- Unity 생성 캐시와 로컬 IDE 파일은 제외하고, 리뷰에 필요한 소스/설정/에셋 중심으로 정리했습니다.

## 기술스택

- Engine: Unity 6000.3.9f1
- Language: C#
- Render Pipeline: Universal Render Pipeline 17.3.0
- Input: Unity Input System 1.18.0
- UI: Unity UI, TextMesh Pro
- Map/2D: Unity 2D Sprite, Tilemap, 2D Tilemap Extras
- Data: ScriptableObject 기반 스탯, 아이템, 적, 바이옴, 보스방 설정
- Version Control: Git, GitHub

## 게임 시스템

### 플레이어

- 이동, 대시, 방향 기반 공격, 카메라 연동 흐름
- 근거리/원거리 기본 공격 처리
- 투사체와 이펙트 런타임 풀링
- 직업별 스킬 컨트롤러와 스킬 쿨타임 UI
- 체력, 사망 처리, 상태이상, HUD 연동

주요 코드:

- `Assets/_Project/Scripts/Gameplay/Player/Runtime/PlayerController.cs`
- `Assets/_Project/Scripts/Gameplay/Player/Runtime/PlayerStats.cs`
- `Assets/_Project/Scripts/Gameplay/Player/Combat/PlayerAttack.cs`
- `Assets/_Project/Scripts/Gameplay/Player/Skills/PlayerClassSkillController.cs`

### 성장과 스탯

- 체력, 이동속도, 공격력, 공격속도, 공격 사거리, 마력, 스킬 쿨타임 감소 스탯 관리
- 레벨업 진행과 스탯 선택 UI 흐름
- 플레이어 런타임 스탯과 아이템 스탯 보정값 분리
- 플레이어/적 전투 계산식 분리
- 직업 선택과 직업별 스킬 해금 흐름

주요 코드:

- `Assets/_Project/Scripts/Gameplay/Player/Progression/LevelUpManager.cs`
- `Assets/_Project/Scripts/Gameplay/Player/Progression/LevelUpStatCatalog.cs`
- `Assets/_Project/Scripts/Gameplay/Player/Progression/PlayerClass.cs`
- `Assets/_Project/Scripts/Core/Stats/CharacterStats.cs`
- `Assets/_Project/Scripts/Core/Stats/PlayerItemStatDefinitions.cs`

### 적과 보스

- Idle, Wander, Chase, Attack, Charge, Return, Dead 상태 기반 적 FSM
- 일반 적 스폰, 엘리트 스폰, 적 전투 계산 분리
- 장, 간, 위, 폐 보스 콘셉트별 패턴 구현
- 보스방 안개, 가두리, 귀환 포털, 보스 사망 후 정리 흐름

주요 코드:

- `Assets/_Project/Scripts/Gameplay/Enemies/Core/EnemyController.cs`
- `Assets/_Project/Scripts/Gameplay/Enemies/FSM/`
- `Assets/_Project/Scripts/Gameplay/Enemies/Spawning/EnemySpawner.cs`
- `Assets/_Project/Scripts/Gameplay/Enemies/Bosses/`
- `Assets/_Project/Scripts/World/Biomes/BossArena/MidBossArenaController.cs`

### 월드, 바이옴, 맵

- 허브룸과 바이옴 포털 흐름
- 바이옴 런타임 매니저와 청크/오브젝트 생성 구조
- 바이옴 설정, 적 스폰 설정, 보스방 설정을 역할별 ScriptableObject로 분리
- 맵과 바이옴 레이아웃은 아직 미완성이고 제작 중입니다.

주요 코드:

- `Assets/_Project/Scripts/World/Hub/`
- `Assets/_Project/Scripts/World/Biomes/Runtime/`
- `Assets/_Project/Scripts/World/Biomes/Configs/`
- `Assets/_Project/Data/BiomeConfigs/`

### UI와 연출

- HP, EXP, 스탯, 스킬 쿨타임 HUD
- 레벨업 선택, 직업 선택, 사망, 게임오버 UI
- 카메라, 빌보드, 스프라이트 정렬 보조 컴포넌트

주요 코드:

- `Assets/_Project/Scripts/UI/HUD/`
- `Assets/_Project/Scripts/UI/LevelUp/`
- `Assets/_Project/Scripts/UI/Death/`
- `Assets/_Project/Scripts/Presentation/Camera/`

## 폴더 구조

```text
Assets/_Project/
  Art/             게임 아트, 타일, 머티리얼, 셰이더
  Audio/           BGM, 플레이어/전투 사운드
  Data/            ScriptableObject 설정 에셋
  Docs/            개발 문서
  Prefabs/         Unity 프리팹
  Resources/       런타임 리소스와 밸런스 데이터
  Scenes/          Unity 씬
  Scripts/         Core, Gameplay, UI, Presentation, World 코드
  Settings/        프로젝트 전용 설정 에셋
Packages/          Unity 패키지 manifest/lock
ProjectSettings/   Unity 프로젝트 설정
docs/archive/      기존 구현 정리와 리뷰 문서 아카이브
```

## 파일 정리 기준

- `Library/`, `Temp/`, `Obj/`, `Logs/`, `UserSettings/` 같은 Unity 생성 폴더는 제외했습니다.
- `.vs/`, `.vscode/`, `.idea/`, `.playwright-mcp/` 같은 로컬 도구/IDE 캐시는 제외했습니다.
- TextMesh Pro 예제 프로젝트와 Unity 튜토리얼/템플릿 리소스처럼 실행에 필요 없는 샘플 리소스는 제외했습니다.
- 기존 루트에 있던 긴 작업 정리 문서는 `docs/archive/`로 이동했습니다.
- Unity 에셋 참조 안정성을 위해 `.meta` 파일은 유지했습니다.

## 실행 방법

1. Unity `6000.3.9f1`을 설치합니다.
2. Unity Hub에서 이 저장소 폴더를 엽니다.
3. `Packages/manifest.json` 기준으로 패키지 복원이 끝날 때까지 기다립니다.
4. `Assets/_Project/Scenes/` 아래 씬을 열어 확인합니다.

## 다음 작업

- 바이옴/맵 레이아웃 완성
- 보스방 구성과 보스 패턴 밸런싱
- 레벨업, 직업 선택, 사망, HUD UI 연출 보강
- 아이템/성장/전투 시스템 검증 케이스 추가
