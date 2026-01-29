# 블로그 포스트 템플릿

## 기술 블로그 - 구현 기록

```html
<h2>TL;DR</h2>
<ul>
  <li>핵심 포인트 1</li>
  <li>핵심 포인트 2</li>
  <li>핵심 포인트 3</li>
</ul>

<h2>배경</h2>
<p>왜 이 작업이 필요했는지 설명합니다.</p>

<h2>구현 과정</h2>

<h3>1단계: [작업명]</h3>
<p>설명</p>

<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>def example():
    pass</code></pre>
<!-- /wp:code -->

<h3>2단계: [작업명]</h3>
<p>설명</p>

<h2>발생한 문제와 해결</h2>

<h3>문제: [문제 설명]</h3>
<p><strong>원인:</strong> 원인 설명</p>
<p><strong>해결:</strong> 해결 방법</p>

<h2>결과</h2>
<ul>
  <li>달성한 것 1</li>
  <li>달성한 것 2</li>
</ul>

<h2>마치며</h2>
<p>배운 점과 다음 단계</p>
```

## 튜토리얼 - 단계별 가이드

```html
<h2>이 글에서 배울 것</h2>
<ul>
  <li>학습 목표 1</li>
  <li>학습 목표 2</li>
</ul>

<h2>사전 준비</h2>
<ul>
  <li>필요한 환경/도구</li>
</ul>

<h2>Step 1: [단계명]</h2>
<p>상세 설명</p>

<!-- wp:code {"language":"bash"} -->
<pre class="wp-block-code"><code>npm install example</code></pre>
<!-- /wp:code -->

<h2>Step 2: [단계명]</h2>
<p>상세 설명</p>

<h2>전체 코드</h2>

<!-- wp:code {"language":"javascript"} -->
<pre class="wp-block-code"><code>// 완성된 코드 전체</code></pre>
<!-- /wp:code -->

<h2>정리</h2>
<p>핵심 요약</p>
```

## 프로젝트 업데이트 - 진행 보고

```html
<h2>이번 업데이트 요약</h2>
<ul>
  <li>완료된 작업 1</li>
  <li>완료된 작업 2</li>
</ul>

<h2>주요 변경 사항</h2>

<h3>기능 1: [기능명]</h3>
<p>설명 및 스크린샷</p>

<h3>기능 2: [기능명]</h3>
<p>설명 및 스크린샷</p>

<h2>다음 단계</h2>
<ul>
  <li>예정된 작업 1</li>
  <li>예정된 작업 2</li>
</ul>
```

## Gutenberg 블록 참조

### 코드 블록
```html
<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>코드 내용</code></pre>
<!-- /wp:code -->
```

### 이미지
```html
<!-- wp:image {"id":123,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large">
  <img src="이미지URL" alt="대체텍스트"/>
  <figcaption>캡션</figcaption>
</figure>
<!-- /wp:image -->
```

### 인용문
```html
<!-- wp:quote -->
<blockquote class="wp-block-quote">
  <p>인용 내용</p>
  <cite>출처</cite>
</blockquote>
<!-- /wp:quote -->
```

### 알림 박스 (Callout)
```html
<!-- wp:paragraph {"backgroundColor":"pale-cyan-blue"} -->
<p class="has-pale-cyan-blue-background-color has-background">
  ℹ️ 정보성 메시지
</p>
<!-- /wp:paragraph -->
```

## 시각적 요소 템플릿

### Mermaid 플로우차트

프로세스, 워크플로우, 의사결정 흐름 표현용:

```html
<!-- wp:html -->
<pre class="mermaid">
flowchart TD
    A[🚀 시작] --> B{조건 확인}
    B -->|조건 충족| C[처리 실행]
    B -->|조건 미충족| D[대안 처리]
    C --> E[✅ 완료]
    D --> E

    style A fill:#e1f5fe
    style E fill:#c8e6c9
</pre>
<!-- /wp:html -->
```

### Mermaid 시퀀스 다이어그램

API 호출, 컴포넌트 간 통신, 이벤트 흐름 표현용:

```html
<!-- wp:html -->
<pre class="mermaid">
sequenceDiagram
    participant C as 클라이언트
    participant S as 서버
    participant D as 데이터베이스

    C->>S: 요청 전송
    S->>D: 데이터 조회
    D-->>S: 결과 반환
    S-->>C: 응답 전송
</pre>
<!-- /wp:html -->
```

### Mermaid 아키텍처 다이어그램

시스템 구조, 컴포넌트 관계 표현용:

```html
<!-- wp:html -->
<pre class="mermaid">
graph TB
    subgraph Frontend
        A[React App]
        B[Redux Store]
    end

    subgraph Backend
        C[API Server]
        D[Auth Service]
    end

    subgraph Database
        E[(PostgreSQL)]
        F[(Redis Cache)]
    end

    A --> C
    B --> A
    C --> D
    C --> E
    C --> F
</pre>
<!-- /wp:html -->
```

### 비교표 (A vs B)

기술 선택, 라이브러리 비교 시 사용:

```html
<!-- wp:table {"className":"is-style-stripes"} -->
<figure class="wp-block-table is-style-stripes">
<table>
  <thead>
    <tr>
      <th>비교 항목</th>
      <th>🅰️ 옵션 A</th>
      <th>🅱️ 옵션 B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>장점</strong></td>
      <td>✅ 빠른 성능<br>✅ 쉬운 학습곡선</td>
      <td>✅ 풍부한 생태계<br>✅ 강력한 타입 지원</td>
    </tr>
    <tr>
      <td><strong>단점</strong></td>
      <td>❌ 제한된 기능<br>❌ 작은 커뮤니티</td>
      <td>❌ 복잡한 설정<br>❌ 높은 러닝커브</td>
    </tr>
    <tr>
      <td><strong>추천 상황</strong></td>
      <td>소규모 프로젝트</td>
      <td>대규모 프로젝트</td>
    </tr>
  </tbody>
</table>
<figcaption>옵션 A와 옵션 B 비교</figcaption>
</figure>
<!-- /wp:table -->
```

### 기능 요약표

구현된 기능, API 엔드포인트 정리용:

```html
<!-- wp:table -->
<figure class="wp-block-table">
<table>
  <thead>
    <tr>
      <th>기능</th>
      <th>설명</th>
      <th>상태</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>🔐 인증</td>
      <td>JWT 기반 사용자 인증</td>
      <td>✅ 완료</td>
    </tr>
    <tr>
      <td>📝 CRUD</td>
      <td>기본 데이터 관리</td>
      <td>✅ 완료</td>
    </tr>
    <tr>
      <td>🔍 검색</td>
      <td>전문 검색 기능</td>
      <td>🚧 진행중</td>
    </tr>
  </tbody>
</table>
<figcaption>기능 구현 현황</figcaption>
</figure>
<!-- /wp:table -->
```

### 인포그래픽 - 핵심 요약 박스

TL;DR, 핵심 포인트 강조용:

```html
<!-- wp:group {"backgroundColor":"cyan-bluish-gray","className":"summary-box"} -->
<div class="wp-block-group summary-box has-cyan-bluish-gray-background-color has-background" style="padding: 1.5rem; border-radius: 8px;">
  <h4 style="margin-top: 0;">💡 핵심 요약</h4>
  <ul style="margin-bottom: 0;">
    <li><strong>문제:</strong> 기존 방식의 한계점</li>
    <li><strong>해결:</strong> 새로운 접근 방식</li>
    <li><strong>결과:</strong> 달성한 성과</li>
  </ul>
</div>
<!-- /wp:group -->
```

### 인포그래픽 - 경고/주의 박스

중요한 주의사항, 경고 메시지용:

```html
<!-- wp:group {"backgroundColor":"vivid-red","className":"warning-box"} -->
<div class="wp-block-group warning-box" style="background: #fff3cd; border-left: 4px solid #ffc107; padding: 1rem; margin: 1rem 0;">
  <h4 style="margin-top: 0; color: #856404;">⚠️ 주의사항</h4>
  <p style="margin-bottom: 0; color: #856404;">이 작업은 되돌릴 수 없습니다. 실행 전 백업을 권장합니다.</p>
</div>
<!-- /wp:group -->
```

### 인포그래픽 - 팁 박스

유용한 팁, 추가 정보 제공용:

```html
<!-- wp:group {"className":"tip-box"} -->
<div class="wp-block-group tip-box" style="background: #d4edda; border-left: 4px solid #28a745; padding: 1rem; margin: 1rem 0;">
  <h4 style="margin-top: 0; color: #155724;">💚 Pro Tip</h4>
  <p style="margin-bottom: 0; color: #155724;">환경변수를 .env 파일로 관리하면 보안과 편의성을 모두 챙길 수 있습니다.</p>
</div>
<!-- /wp:group -->
```

### 단계별 프로세스 카드

튜토리얼, 가이드의 각 단계 시각화:

```html
<!-- wp:group {"className":"step-cards"} -->
<div class="wp-block-group step-cards">

  <!-- Step 1 -->
  <div style="border-left: 4px solid #0073aa; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #0073aa;">📌 Step 1: 프로젝트 초기화</h4>
    <p style="margin: 0.5rem 0 0;">npm init으로 새 프로젝트를 생성합니다.</p>
  </div>

  <!-- Step 2 -->
  <div style="border-left: 4px solid #0073aa; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #0073aa;">📌 Step 2: 의존성 설치</h4>
    <p style="margin: 0.5rem 0 0;">필요한 패키지를 설치합니다.</p>
  </div>

  <!-- Step 3 -->
  <div style="border-left: 4px solid #28a745; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #28a745;">✅ Step 3: 완료</h4>
    <p style="margin: 0.5rem 0 0;">설정이 완료되었습니다.</p>
  </div>

</div>
<!-- /wp:group -->
```

### 통계/수치 하이라이트

성능 개선, 결과 수치 강조용:

```html
<!-- wp:columns -->
<div class="wp-block-columns">

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #e3f2fd; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #1976d2; margin: 0;">50%</p>
    <p style="margin: 0; color: #666;">성능 향상</p>
  </div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #e8f5e9; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #388e3c; margin: 0;">3x</p>
    <p style="margin: 0; color: #666;">처리 속도</p>
  </div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #fff3e0; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #f57c00; margin: 0;">99.9%</p>
    <p style="margin: 0; color: #666;">가용성</p>
  </div>
  <!-- /wp:column -->

</div>
<!-- /wp:columns -->
```
