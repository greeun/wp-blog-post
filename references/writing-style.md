# Writing Style — AI Slop Prevention (CRITICAL)

SKILL.md "Writing Style Rules → AI Slop Prevention"의 상세. 초안 작성 시, 그리고 실행 절차 7단계(발행 전 Self-Check)에서 이 파일을 Read한다. 한국어 종결어미 마침표 규칙은 SKILL.md에 그대로 있다.

## AI Slop Prevention (CRITICAL)

단일 패스 생성은 일반 튜토리얼처럼 균질화되기 쉽다. 아래 규칙을 위반한 초안은 발행하지 않는다.

### 금지 오프닝 (Banned Openings)

| 언어 | 금지 패턴 |
|---|---|
| KO | `바야흐로 ~` |
| KO | `오늘날 빠르게 변화하는 ~` / `급변하는 ~ 시대` |
| KO | `이번 포스트에서는 ~에 대해 알아보겠습니다` |
| KO | `~는 현대 개발의 필수 ~` |
| KO | `AI가 세상을 바꾸는 ~` |
| EN | `In today's fast-paced world, ~` |
| EN | `In this post, we will explore ~` |
| EN | `~ has become essential in modern development` |
| EN | `Let's dive into ~` |

대신 **이 세션에서 실제로 마주친 문제·결과**로 연다.
- Good: "FastAPI 0.110에서 `lifespan` 이벤트로 바꾸다가 `startup` 콜백이 2회 실행되는 문제를 만났다."
- Bad: "오늘날 빠르게 변화하는 개발 환경에서 FastAPI는 필수 프레임워크가 되었습니다."

### 금지 filler (Banned Filler Phrases)

등장하면 삭제하거나 구체 표현으로 대체한다.

- 수식어: `혁신적인`, `놀라운`, `강력한`, `효율적인`, `최신의`
- 양화: `다양한 ~`, `수많은 ~`, `여러 가지 ~` → 실제 개수·이름으로
- 단정: `매우 중요합니다`, `꼭 필요합니다`, `핵심입니다` → 중요한 이유를 쓰거나 삭제
- 완충: `~라고 할 수 있습니다`, `~라고 볼 수 있습니다` → 근거 제시 또는 단정
- 전환: `자, 이제 ~`, `다음으로 ~`, `마지막으로 ~` → 제목으로 대체

### 가짜 코드 금지 (No Placeholder Code)

- `doSomething()`, `foo`, `bar`, `...`, `TODO`, `// 생략`, `pass # 구현 예정` 금지
- 코드 블록의 함수·변수는 이 세션에서 실제로 쓴 것과 이름이 일치해야 함
- 불가피한 축약은 `# ... (위와 동일한 import)` 처럼 **축약 의도를 명시**
- 모든 코드 블록에 language tag (` ```python `, ` ```bash ` 등) 필수

### 구체성 강제 (Specificity Over Generality)

- 구체 수치·버전·에러 메시지·파일 경로를 포함한다.
  - ❌ "성능이 개선되었다"
  - ✅ "p95 응답 시간이 820ms → 190ms로 줄었다"
- 이 세션에서만 얻은 관찰(**insight angle**)을 최소 1건 포함한다.
  - 예: "문서화된 기본 TTL 60초와 달리 내부적으로 600초로 강제된다."
- Stack Overflow 복사 수준의 일반 패턴만 나열하면 실패.

### 발행 전 Self-Check (MANDATORY)

`publish_post.py` 호출 **전에** 초안을 훑어 아래 전부를 확인한다. 하나라도 실패하면 중단하고 수정.

- [ ] 오프닝 1문장이 `금지 오프닝` 목록에 없음
- [ ] `혁신적인`·`놀라운`·`다양한`·`매우 중요` 등 filler **0건** (grep으로 전수조사)
- [ ] `doSomething`·`foo`·`bar`·`TODO`·`...` 플레이스홀더 코드 **0건**
- [ ] 모든 코드 블록에 language tag 존재
- [ ] 이 세션 고유의 구체 디테일(수치/버전/에러/경로) **최소 3건**
- [ ] 한국어 종결어미 마침표 누락 **0건** (아래 grep)
- [ ] 2개 이상 섹션에서 같은 주장을 다른 표현으로 반복하지 않음

빠른 검사용 grep:

```bash
# KO 종결어미 마침표 누락 (다/했다/된다/있다/없다/한다/이다 직후 마침표 없음)
grep -nE "(다|했다|된다|있다|없다|한다|이다)([ 	]*$|[ 	]+[^.．。])" /tmp/post_content.html

# Filler 전수조사
grep -niE "혁신적인|놀라운|강력한|다양한|수많은|매우 중요|꼭 필요|~라고 할 수|~라고 볼 수" /tmp/post_content.html

# Placeholder 코드
grep -nE "doSomething|\\bfoo\\b|\\bbar\\b|TODO|\\.\\.\\.|// 생략" /tmp/post_content.html
```
