# Changelog

## [1.1.0] — 2026-06-17

인포그래픽 우선(infographic-first) 시각화 지침 추가. 포스트 구조와 각 설명이 한눈에
들어오도록 시각 요소를 본문 전달의 1차 수단으로 사용하고, 모든 인포그래픽을 플러그인
비의존 정적 이미지로 생성하도록 규정.

### Added
- **Infographic-First Principle (MANDATORY)** 섹션 (`SKILL.md` §5)
  - 마인드맵·다이어그램/도식·순서도·아키텍처·차트/그래프·타임라인·시퀀스·웹툰/일러스트 등
    시각 유형 확대 (열거 목록은 예시일 뿐 비고정 — 그 외 창의적 시각화도 허용)
  - "주요 섹션마다 시각 요소 1개" 권장, "이미지만 훑어도 글 이해 가능한가" 기준
- **Plugin-Independent Images (REQUIRED)** — 모든 인포그래픽은 자체 완결 정적 이미지
  (PNG/JPG/SVG)로 렌더 후 코어 `wp:image` 블록 삽입. 인라인 Mermaid·숏코드·플러그인 전용
  블록·JS 차트 라이브러리 금지 (플러그인 0개 환경에서도 동일 렌더)
- `mmdc` 멀티 타입 렌더 안내 (mindmap·xychart-beta·pie·timeline·quadrantChart)
- 웹툰/커스텀 인포그래픽: 이미지 생성 → `upload_media.py` 업로드 경로

## [1.0.0] — 2026-04-22

첫 버저닝 릴리스. 기존 비버저닝 상태(6개 커밋)를 안정 버전으로 고정하고,
단일 패스 작성에서도 AI slop을 차단하기 위한 Writing Style 규칙을 추가.

### Added
- **AI Slop Prevention** 섹션 (`SKILL.md`)
  - 금지 오프닝 목록 (KO/EN 9종): "바야흐로", "오늘날 빠르게 변화하는",
    "In today's fast-paced world" 등
  - 금지 filler 문구: "혁신적인", "놀라운", "다양한", "매우 중요" 등
  - 가짜 코드 금지: `doSomething`, `foo`, `bar`, `TODO`, `...` 플레이스홀더 차단
  - 구체성 강제: insight angle 최소 1건 포함, 구체 수치·버전·에러·경로 사용
  - 발행 전 Self-Check 7개 체크리스트 + grep 명령어 3종
    (종결어미 마침표 / filler / placeholder 코드)
- `Execution Procedure` 7단계에 "Run AI Slop self-check" 삽입 (8→9 단계)
- `VERSION` 파일 및 `CHANGELOG.md` 최초 도입

### Context
- 자매 스킬 `wp-blog-post-harness`는 Evaluator 서브에이전트로 AI slop을
  능동 탐지하지만, 본 스킬은 단일 패스이므로 **작성자 self-check**로 이식.
- 방어 강도는 harness 버전보다 약함. 엄격한 품질 검증이 필요하면
  `wp-blog-post-harness` 사용 권장.

### Prior unversioned history
- `79336fe` fix: Gutenberg 블록 오류 및 Mermaid 다이어그램 렌더링 수정
- `5f547a2` fix: 한국어 종결어미 마침표 규칙 추가
- `ffbacab` fix: 한글 깨짐 방지 (`ensure_ascii=False`)
- `db2fa17` feat: 시각 요소 추가, 문서 영문화
- `89fc1ec` feat: 카테고리·태그 자동 생성
- `b48d9c8` feat: 초기 WordPress 블로그 포스트 스킬
