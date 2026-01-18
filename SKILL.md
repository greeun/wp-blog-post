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
1. 세션 내용 분석 → 2. 포스트 구조 설계 → 3. 콘텐츠 생성 → 4. 워드프레스 발행
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

## 3. 포스트 템플릿

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

## 4. 워드프레스 발행

### 발행 스크립트 사용

```bash
python .claude/skills/wp-blog-post/scripts/publish_post.py \
  --title "포스트 제목" \
  --content-file /tmp/post_content.html \
  --status draft \
  --categories "개발,기술" \
  --tags "python,claude"
```

### 옵션

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `--status` | draft / publish | draft |
| `--categories` | 카테고리 (쉼표 구분) | - |
| `--tags` | 태그 (쉼표 구분) | - |
| `--featured-image` | 대표 이미지 경로 | - |

## 5. 실행 절차

1. **콘텐츠 파일 생성**: 마크다운을 HTML로 변환하여 `/tmp/post_content.html`에 저장
2. **환경변수 확인**: `WP_SITE_URL`, `WP_USERNAME`, `WP_APP_PASSWORD`
3. **발행 스크립트 실행**: 초안으로 저장
4. **결과 확인**: 반환된 포스트 URL 제공

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
