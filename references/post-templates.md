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
