# WordPress Blog Post Skill

Claude Code 작업 세션 내용을 워드프레스 블로그 포스트로 자동 변환하고 발행하는 스킬입니다.

## 설치

### 1. 환경변수 설정

`~/.zshrc` 또는 `~/.bashrc`에 추가:

```bash
export WP_SITE_URL="https://your-site.com"
export WP_USERNAME="your-username"
export WP_APP_PASSWORD="xxxx xxxx xxxx xxxx xxxx xxxx"
```

설정 후 터미널 재시작 또는 `source ~/.zshrc` 실행.

### 2. Application Password 생성

1. 워드프레스 관리자 로그인
2. **사용자** → **프로필** 이동
3. 페이지 하단 **애플리케이션 비밀번호** 섹션
4. 이름 입력 (예: "Claude Code") → **새 애플리케이션 비밀번호 추가**
5. 생성된 비밀번호를 `WP_APP_PASSWORD`에 설정

> Application Password는 WordPress 5.6+ 기본 기능입니다. 이전 버전은 플러그인 필요.

## 사용법

### Claude Code에서 스킬 호출

다음과 같이 요청하면 스킬이 자동으로 활성화됩니다:

```
블로그 작성해줘
작업 내용 워드프레스에 올려줘
기술 블로그로 정리해줘
튜토리얼 작성해줘
세션 정리해서 블로그로
```

### 스크립트 직접 사용

#### 포스트 발행

```bash
# 마크다운 파일을 HTML로 변환
python .claude/skills/wp-blog-post/scripts/md_to_html.py \
  --input post.md \
  --output /tmp/post.html

# 워드프레스에 발행 (초안)
python .claude/skills/wp-blog-post/scripts/publish_post.py \
  --title "포스트 제목" \
  --content-file /tmp/post.html \
  --status draft \
  --categories "개발,기술" \
  --tags "python,automation"
```

#### 옵션

| 옵션 | 설명 | 예시 |
|------|------|------|
| `--title` | 포스트 제목 (필수) | `"Claude Code 활용기"` |
| `--content` | HTML 콘텐츠 직접 입력 | `"<p>내용</p>"` |
| `--content-file` | HTML 파일 경로 | `/tmp/post.html` |
| `--status` | 발행 상태 | `draft`, `publish`, `pending`, `private` |
| `--categories` | 카테고리 (쉼표 구분) | `"개발,기술블로그"` |
| `--tags` | 태그 (쉼표 구분) | `"python,claude,ai"` |
| `--featured-media` | 대표 이미지 ID | `123` |

#### 이미지 업로드

```bash
python .claude/skills/wp-blog-post/scripts/upload_media.py \
  --file ./screenshot.png \
  --alt-text "스크린샷 설명" \
  --caption "이미지 캡션"
```

출력:
```json
{
  "success": true,
  "id": 456,
  "url": "https://your-site.com/wp-content/uploads/2024/01/screenshot.png"
}
```

반환된 `id`를 `--featured-media`에 사용할 수 있습니다.

## 포스트 템플릿

### 기술 블로그

```markdown
# 제목

## TL;DR
- 핵심 포인트 1
- 핵심 포인트 2

## 배경
왜 이 작업이 필요했는지

## 구현 과정

### 1단계: 작업명
설명과 코드

### 2단계: 작업명
설명과 코드

## 발생한 문제와 해결

### 문제: 설명
- **원인**:
- **해결**:

## 결과
달성한 것들

## 마치며
배운 점
```

### 튜토리얼

```markdown
# ~하는 방법

## 이 글에서 배울 것
- 목표 1
- 목표 2

## 사전 준비
- 필요 환경

## Step 1: 단계명
설명 + 코드

## Step 2: 단계명
설명 + 코드

## 전체 코드
완성 코드

## 정리
요약
```

## 파일 구조

```
wp-blog-post/
├── SKILL.md                 # 스킬 정의 (Claude Code용)
├── README.md                # 이 문서
├── scripts/
│   ├── publish_post.py      # 포스트 발행 API
│   ├── upload_media.py      # 미디어 업로드 API
│   └── md_to_html.py        # 마크다운→HTML 변환
└── references/
    └── post-templates.md    # Gutenberg 블록 템플릿
```

## 문제 해결

### "Missing environment variables" 오류

환경변수가 설정되지 않았습니다:
```bash
echo $WP_SITE_URL
echo $WP_USERNAME
echo $WP_APP_PASSWORD
```

빈 값이면 환경변수를 다시 설정하세요.

### "401 Unauthorized" 오류

- Application Password가 올바른지 확인
- 워드프레스에서 REST API가 활성화되어 있는지 확인
- 사용자에게 포스트 작성 권한이 있는지 확인

### "Could not create category" 경고

카테고리 생성 권한이 없을 수 있습니다. 기존 카테고리 이름을 사용하거나 관리자에게 권한 요청하세요.

### REST API 비활성화

일부 보안 플러그인이 REST API를 차단합니다:
- Wordfence, iThemes Security 등의 설정 확인
- `/wp-json/wp/v2/posts` 엔드포인트 접근 허용 필요

## 보안 참고사항

- Application Password는 안전하게 보관하세요
- `.bashrc`/`.zshrc` 파일 권한을 `600`으로 설정 권장
- 공용 환경에서는 환경변수 대신 별도 credential 파일 사용 고려
