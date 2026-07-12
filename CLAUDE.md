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

## 패치 이력 (점진 패치 노선 — 상세는 GAME_DESIGN/TECH_SPEC 패치 로그)

- v1.1 (07-10): 위험 경고(바닥 72%, 주황 점선) · 콤보 타건음 피치 · HUD 레벨 진행 도트
- v1.2 (07-11): 직전 판 성장 비교(▲▼) · 완주 시 다음 난이도 도전 버튼 · 첫 판 타겟팅 온보딩 힌트
- v1.3 (07-11): 🏅 오늘의 도전 — 날짜+난이도 시드 결정론 RNG(mulberry32)로 반 전원 동일 문제 순서(대항전). 난이도 화면 C키 토글, 결과·복사 태그. probe.html에 결정론(A===B) 검증 추가.
- v1.4 (07-12): 🏅 오늘의 도전 랭킹 수합기 `teacher-ranking.html`(교사용, 별도 파일) — 학생 결과 복사 텍스트 붙여넣기→파싱→최고점 순위표+필터+TSV 복사. probe-ranking으로 검증(임시, 삭제됨).

## 검증

- 헤드리스 iframe 프로브 (TECH_SPEC §8 시나리오 15종 기준): probe.html로 index.html을 감싸 keydown 디스패치 →
  `msedge --headless=new --allow-file-access-from-files --virtual-time-budget=20000 --dump-dom`.
- ⚠️ ~~rAF 정지 한계~~ → **해결(2026-07-10)**: `probe.html`이 iframe에 rAF를 setTimeout(16ms) 심으로 교체해 낙하·스폰·레벨업·위험경고·바닥판정까지 자동 검증한다. 패치 후 반드시 probe.html 실행.

## 형제 프로젝트

정글(jgle.kr)의 CLAUDE.md가 전체 지도. 이 게임의 디자인 톤은 GAME_DESIGN §9(이펙트 절제) — 파티클·화면 흔들림·BGM 금지 유지.
