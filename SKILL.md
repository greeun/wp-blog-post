---
name: wp-blog-post
description: |
  작업 세션 내용을 워드프레스 블로그 포스트로 작성하고 발행.
  "블로그 작성", "포스트 발행", "워드프레스에 올려", "기술 블로그", "튜토리얼 작성",
  "세션 정리해서 블로그로", "작업 내용 블로그화" 요청 시 사용.
---

# WordPress Blog Post Writer

작업 세션 내용을 분석하여 워드프레스 블로그 포스트를 자동 생성하고 발행합니다.

## 워크플로우

```
1. 세션 내용 분석 → 2. 카테고리 자동 선택 → 3. 태그 자동 생성 → 4. 포스트 구조 설계 → 5. 콘텐츠 생성 → 6. 워드프레스 발행
```

## 1. 환경 설정 확인

워드프레스 연동을 위해 환경변수가 필요합니다:

```bash
# ~/.zshrc 또는 ~/.bashrc에 추가
export WP_SITE_URL="https://your-site.com"
export WP_USERNAME="your-username"
export WP_APP_PASSWORD="xxxx xxxx xxxx xxxx xxxx xxxx"
```

**Application Password 생성 방법:**
1. 워드프레스 관리자 → 사용자 → 프로필
2. "애플리케이션 비밀번호" 섹션에서 새 비밀번호 생성

## 2. 세션 내용 분석

현재 세션에서 다음을 추출:

| 항목 | 추출 내용 |
|------|----------|
| **주제** | 작업한 기능/버그/개선 사항 |
| **코드 변경** | 수정/추가된 주요 코드 |
| **문제 해결** | 발생한 문제와 해결 방법 |
| **학습 포인트** | 새로 알게 된 내용 |
| **카테고리** | 기존 카테고리에서 자동 선택 또는 신규 생성 |
| **태그** | 콘텐츠 기반 자동 생성 태그 |

## 3. 카테고리 자동 선택

콘텐츠를 분석하여 적절한 카테고리를 자동으로 선택하거나 새로 생성합니다.

### 카테고리 조회

발행 전 기존 카테고리 목록을 조회합니다:

```bash
# 기존 카테고리 목록 조회
curl -s "${WP_SITE_URL}/wp-json/wp/v2/categories?per_page=100" | \
  python3 -c "import sys,json; cats=json.load(sys.stdin); print('\n'.join(f\"{c['id']}: {c['name']} ({c['count']}개 포스트)\" for c in cats))"
```

### 카테고리 선택 규칙

1. **기존 카테고리 우선 매칭**
   - 콘텐츠 주제와 가장 관련 있는 기존 카테고리 선택
   - 여러 카테고리가 해당되면 1-2개 선택

2. **신규 카테고리 생성 기준**
   - 기존 카테고리 중 적합한 것이 없을 때만 생성
   - 향후 재사용 가능성이 높은 주제일 때 생성
   - 너무 세분화된 카테고리는 피함

3. **계층 구조 활용**
   - 상위 카테고리가 있다면 하위 카테고리로 분류
   - 예: `개발` > `백엔드`, `개발` > `프론트엔드`

### 카테고리 매핑 예시

| 콘텐츠 주제 | 선택/생성 카테고리 |
|------------|-------------------|
| React 컴포넌트 개발 | `프론트엔드` 또는 `Frontend` |
| FastAPI REST API | `백엔드` 또는 `Backend` |
| Docker 배포 자동화 | `DevOps` 또는 `인프라` |
| 버그 수정 경험 | `트러블슈팅` 또는 `Troubleshooting` |
| 새 라이브러리 사용법 | `튜토리얼` 또는 `Tutorial` |
| 개발 환경 설정 | `개발환경` 또는 `Setup` |
| AI/ML 관련 작업 | `AI/ML` 또는 `인공지능` |

### 카테고리 생성 가이드라인

- **1-2개** 카테고리 선택 (너무 많으면 분류 의미 상실)
- 기존 카테고리 네이밍 컨벤션 따르기 (한글/영문 통일)
- 포스트 수가 적은 카테고리는 통합 고려
- 신규 생성 시 비슷한 기존 카테고리가 없는지 재확인

## 4. 태그 자동 생성

콘텐츠를 분석하여 관련 태그를 자동으로 생성합니다.

### 태그 추출 규칙

1. **기술 스택 태그**: 사용된 언어, 프레임워크, 라이브러리
   - 예: `python`, `react`, `typescript`, `fastapi`, `docker`

2. **주제 태그**: 다루는 기술 영역
   - 예: `api`, `database`, `authentication`, `testing`, `devops`

3. **작업 유형 태그**: 수행한 작업의 성격
   - 예: `tutorial`, `troubleshooting`, `performance`, `refactoring`, `bug-fix`

4. **도구/서비스 태그**: 사용한 외부 도구나 서비스
   - 예: `github-actions`, `aws`, `claude-code`, `vscode`

### 태그 생성 가이드라인

- **5-10개** 태그를 목표로 생성
- **영문 태그**: 소문자와 하이픈 사용 (예: `claude-code`, `rest-api`)
- **한글 태그**: 검색 친화적인 한글 키워드 포함 (예: `파이썬`, `웹개발`)
- **영문 + 한글 혼합**: 동일 개념에 대해 영문/한글 태그 모두 포함 권장
- 너무 일반적인 태그 회피 (예: `code`, `프로그래밍`)
- 검색 가능성을 고려하여 구체적인 태그 선택
- 콘텐츠에 실제로 등장하는 기술만 태그로 사용

### 자동 태그 예시

| 콘텐츠 주제 | 생성되는 태그 |
|------------|--------------|
| React에서 상태 관리 | `react`, `리액트`, `state-management`, `상태관리`, `hooks`, `frontend`, `프론트엔드` |
| Python FastAPI CRUD | `python`, `파이썬`, `fastapi`, `rest-api`, `crud`, `backend`, `백엔드`, `API개발` |
| Docker 컨테이너 배포 | `docker`, `도커`, `container`, `컨테이너`, `deployment`, `배포`, `devops` |
| 인증 시스템 구현 | `authentication`, `인증`, `jwt`, `security`, `보안`, `login`, `로그인` |

## 5. 포스트 템플릿

### 기술 블로그 형식

```markdown
# [제목: 무엇을 했는지 명확하게]

## TL;DR
- 핵심 포인트 3-5개 bullet

## 배경
왜 이 작업이 필요했는지

## 구현 과정

### 1단계: [첫 번째 작업]
설명과 코드 스니펫

### 2단계: [두 번째 작업]
설명과 코드 스니펫

## 발생한 문제와 해결

### 문제 1: [문제 설명]
- **원인**:
- **해결**:

## 결과
- 달성한 것
- 스크린샷/데모 (있다면)

## 마치며
배운 점, 다음 단계
```

### 튜토리얼 형식

```markdown
# [제목: ~하는 방법]

## 이 글에서 배울 것
- 학습 목표 나열

## 사전 준비
- 필요한 환경/도구

## Step 1: [첫 번째 단계]
상세 설명 + 코드

## Step 2: [두 번째 단계]
상세 설명 + 코드

## 전체 코드
완성된 코드 전체

## 정리
핵심 요약
```

## 6. 워드프레스 발행

### 발행 스크립트 사용

```bash
# 카테고리: 기존 목록에서 자동 선택 또는 신규 생성
# 태그: 콘텐츠 분석 후 자동 생성 (영문 + 한글 혼합)
python .claude/skills/wp-blog-post/scripts/publish_post.py \
  --title "FastAPI로 REST API 구축하기" \
  --content-file /tmp/post_content.html \
  --status draft \
  --categories "백엔드,튜토리얼" \
  --tags "python,파이썬,fastapi,rest-api,API개발,backend,백엔드,tutorial,튜토리얼"
```

### 옵션

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--status` | draft / publish | draft |
| `--categories` | 카테고리 (쉼표 구분, 자동 선택) | - |
| `--tags` | 태그 (쉼표 구분, 자동 생성) | - |
| `--featured-image` | 대표 이미지 경로 | - |

**참고**:
- `--categories`는 기존 카테고리에서 자동 선택되거나 필요시 신규 생성됩니다.
- `--tags`는 콘텐츠 분석을 통해 자동으로 생성됩니다 (영문 + 한글).
- 수동 지정도 가능합니다.

## 7. 실행 절차

1. **콘텐츠 파일 생성**: 마크다운을 HTML로 변환하여 `/tmp/post_content.html`에 저장
2. **기존 카테고리 조회**: 워드프레스에서 카테고리 목록 가져오기
3. **카테고리 자동 선택**: 콘텐츠에 맞는 기존 카테고리 선택 또는 신규 생성 결정
4. **태그 자동 생성**: 콘텐츠 분석하여 관련 태그 5-10개 추출 (영문 + 한글)
5. **환경변수 확인**: `WP_SITE_URL`, `WP_USERNAME`, `WP_APP_PASSWORD`
6. **발행 스크립트 실행**: 자동 선택된 카테고리와 태그로 초안 저장
7. **결과 확인**: 반환된 포스트 URL 제공

## 코드 블록 처리

워드프레스에서 코드 하이라이팅을 위해:

```html
<!-- Prism.js 또는 highlight.js 사용 시 -->
<pre><code class="language-python">
def example():
    pass
</code></pre>

<!-- Gutenberg 코드 블록 -->
<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>def example():
    pass</code></pre>
<!-- /wp:code -->
```

## 이미지 처리

로컬 이미지가 있다면 먼저 워드프레스에 업로드 후 URL 사용:

```bash
python .claude/skills/wp-blog-post/scripts/upload_media.py \
  --file ./screenshot.png \
  --alt-text "스크린샷 설명"
```
