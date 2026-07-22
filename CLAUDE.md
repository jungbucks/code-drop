# 코드 낙하 (Code Drop) — 유지보수 플레이북 (후임 모델용 진입점)

> Python 타이핑 아케이드 게임 (교실 오프라인용, 정글 연계). **규칙·수치의 원본은 `GAME_DESIGN.md` + `TECH_SPEC.md`** — 이 두 문서에 없는 규칙을 발명하지 말 것. 충돌 시 GAME_DESIGN이 우선.

## 구조 (2파일 배포)

- `index.html` — 게임 전체(단일 파일). `snippets.js` — 문항 데이터(교사 수정 지점, 스키마는 파일 상단 주석).
- 배포 = 폴더째 복사(오프라인 더블클릭). git 저장소는 2026-07-10 초기화(로컬 이력용, 원격 없음).

## 절대 규칙 (TECH_SPEC §1 요약 + 세션 확정 사항)

1. 프레임워크·빌드·웹폰트·CDN·canvas 금지. 블록은 DOM.
2. GAME_DESIGN의 수치(난이도 표·점수식·등급표)는 CONFIG 상수로만 — 변경은 사용자 지시 + 두 문서 동기 갱신 필수.
3. PLAYING 중 일시정지는 **ESC 전용** ('p'는 코드 글자 — 2026-07-09 확정 편차, TECH_SPEC §4).
4. 학번·이름은 localStorage 저장 금지(세션만). 기록 키: codedrop_best(최고) · codedrop_last(직전 판, v1.2) · codedrop_settings · codedrop_seen(온보딩 플래그) — 전부 이름 없이.
5. 난이도 4종(2026-07-10 비기너 추가): 숫자키 1=비기너 2=초급 3=중급 4=고급. 스니펫 level 0~3.
6. 페이싱(2026-07-10): spawnFactor 0.6 + 화면에 블록 0개면 스폰 대기 0.6초 클램프(`CONFIG.emptyRespawn`).

## 현재 상태 (v1.8) — 상세 패치 로그는 GAME_DESIGN/TECH_SPEC

핵심 루프(4난이도 낙하 타이핑) + 부가 기능:
- **오늘의 도전**: 날짜+난이도 시드 결정론 RNG(mulberry32)로 반 전원 동일 문제 순서. 난이도 화면 C키 토글, 교사용 랭킹 수합기 `teacher-ranking.html`(별도).
- **고스트 레이스**(직전 판 점수 곡선 실시간 비교, `codedrop_last.curve`) · **오답 리포트**(스니펫별 오타 TOP3 + snippets.js `exp` 해설).
- **보스 스니펫**(레벨업 직후 1개, 느리게·×2, CONFIG.boss) · **연습 모드**(P키, 낙하 정지·무기록 — GAME_DESIGN §6 "연습 모드 금지"를 사용자 지시로 개정).
- **낮/밤 테마**(`codedrop_settings.theme`, `:root[data-theme]`) · **예약어 연습**(낙하 없는 `#screen-kw`, 파이썬 예약어 35개 타자 드릴, 무기록).

## 검증

- 헤드리스 iframe 프로브(TECH_SPEC §8, 15종 기준): probe.html이 index.html을 감싸 keydown 디스패치 → `msedge --headless=new --allow-file-access-from-files --virtual-time-budget=20000 --dump-dom`. probe.html이 rAF를 setTimeout(16ms) 심으로 교체해 낙하·스폰·레벨업·위험경고까지 자동 검증. 패치 후 필수.
- ⚠️ **예약어 연습(kw) 검증은 `--virtual-time-budget=400000` 필요**(게임오버 방치 루프 때문).

## 형제 프로젝트

정글(jgle.kr)의 CLAUDE.md가 전체 지도. 이 게임의 디자인 톤은 GAME_DESIGN §9(이펙트 절제) — 파티클·화면 흔들림·BGM 금지 유지.
