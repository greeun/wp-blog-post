---
name: wp-blog-post
description: |
  Convert work session content into WordPress blog posts and publish.
  Use when: "write blog", "publish post", "post to wordpress", "tech blog", "write tutorial",
  "summarize session as blog", "turn work into blog post"
---

# WordPress Blog Post Writer

Analyzes work session content to automatically generate and publish WordPress blog posts.

## Workflow

```
1. Analyze session → 2. Auto-select categories → 3. Auto-generate tags → 4. Design post structure → 5. Create visual elements → 6. Generate content → 7. Publish to WordPress
```

## 1. Environment Setup

WordPress integration requires environment variables:

```bash
# Add to ~/.zshrc or ~/.bashrc
export WP_SITE_URL="https://your-site.com"
export WP_USERNAME="your-username"
export WP_APP_PASSWORD="xxxx xxxx xxxx xxxx xxxx xxxx"
```

**Creating Application Password:**
1. WordPress Admin → Users → Profile
2. Generate new password in "Application Passwords" section

## 2. Session Content Analysis

Extract from current session:

| Item | Extracted Content |
|------|-------------------|
| **Topic** | Features/bugs/improvements worked on |
| **Code Changes** | Key code modified/added |
| **Problem Solving** | Issues encountered and solutions |
| **Learning Points** | New knowledge gained |
| **Categories** | Auto-selected from existing or newly created |
| **Tags** | Auto-generated based on content |

## 3. Auto Category Selection

Analyzes content to automatically select appropriate categories or create new ones.

### Category Query

Query existing categories before publishing:

```bash
# List existing categories
curl -s "${WP_SITE_URL}/wp-json/wp/v2/categories?per_page=100" | \
  python3 -c "import sys,json; cats=json.load(sys.stdin); print('\n'.join(f\"{c['id']}: {c['name']} ({c['count']} posts)\" for c in cats))"
```

### Category Selection Rules

1. **Prioritize existing categories**
   - Select most relevant existing category for content topic
   - Select 1-2 categories if multiple apply

2. **New category creation criteria**
   - Only create when no suitable existing category exists
   - When topic has high reuse potential
   - Avoid overly granular categories

3. **Use hierarchy**
   - Classify under parent category if exists
   - Example: `Development` > `Backend`, `Development` > `Frontend`

### Category Mapping Examples

| Content Topic | Selected/Created Category |
|---------------|---------------------------|
| React component development | `Frontend` |
| FastAPI REST API | `Backend` |
| Docker deployment automation | `DevOps` or `Infrastructure` |
| Bug fix experience | `Troubleshooting` |
| New library usage | `Tutorial` |
| Development environment setup | `Setup` or `DevEnv` |
| AI/ML related work | `AI/ML` |

### Category Guidelines

- Select **1-2 categories** (too many loses classification meaning)
- Follow existing naming conventions
- Consider merging categories with few posts
- Double-check for similar existing categories before creating new

## 4. Auto Tag Generation

Analyzes content to automatically generate relevant tags.

### Tag Extraction Rules

1. **Tech stack tags**: Languages, frameworks, libraries used
   - Example: `python`, `react`, `typescript`, `fastapi`, `docker`

2. **Topic tags**: Technical domains covered
   - Example: `api`, `database`, `authentication`, `testing`, `devops`

3. **Work type tags**: Nature of work performed
   - Example: `tutorial`, `troubleshooting`, `performance`, `refactoring`, `bug-fix`

4. **Tool/service tags**: External tools or services used
   - Example: `github-actions`, `aws`, `claude-code`, `vscode`

### Tag Generation Guidelines

- Target **5-10 tags**
- **English tags**: lowercase with hyphens (e.g., `claude-code`, `rest-api`)
- Avoid overly generic tags (e.g., `code`, `programming`)
- Consider searchability with specific tags
- Only tag technologies actually appearing in content

### Auto Tag Examples

| Content Topic | Generated Tags |
|---------------|----------------|
| React state management | `react`, `state-management`, `hooks`, `frontend` |
| Python FastAPI CRUD | `python`, `fastapi`, `rest-api`, `crud`, `backend` |
| Docker container deployment | `docker`, `container`, `deployment`, `devops` |
| Auth system implementation | `authentication`, `jwt`, `security`, `login` |

## 5. Visual Elements Guidelines

### Infographic-First Principle (MANDATORY)

**The post's structure and every explanation must be graspable at a glance.** Lean on
infographics as the PRIMARY way to communicate, not as decoration. Whenever an idea can be
shown instead of written, show it as an image. Maximize the use of:

- **Mind maps** — topic decomposition, "what this post covers" overview (Mermaid `mindmap`)
- **Diagrams / schematics** — system boundaries, how pieces relate (`graph`, `flowchart`)
- **Flowcharts** — processes, decision logic, request lifecycles
- **Architecture diagrams** — components, data flow, deployment layout
- **Charts & graphs** — before/after metrics, distributions, trends (Mermaid `xychart-beta`, `pie`, `quadrantChart`)
- **Timelines** — version history, migration steps (Mermaid `timeline`)
- **Sequence diagrams** — API calls, multi-actor flows
- **Webtoon / illustrated panels** — narrate a problem→solution story as an AI-generated
  illustration when a concept is more memorable as a scene than a chart
- **Other creative visualizations** — the types above are examples, not an exhaustive list; any
  creative visual that fits the content is welcome (comparison cards, a labeled "map", a quadrant,
  a layered stack, a numbered journey strip, a custom infographic, …)

**Rule of thumb: every major section should carry at least one infographic** so a reader
skimming only the images still understands the post. Default to a visual; fall back to plain
prose only when a visual genuinely adds nothing.

### Plugin-Independent Images (REQUIRED)

**Every infographic must be a self-contained image file (PNG/JPG/SVG) that renders with no
WordPress plugin, shortcode, or client-side library.** A post must look identical on any
WordPress install, even with zero plugins active.

- **Render ahead of time, then embed as a plain image.** Produce the final raster/vector
  file first (Mermaid via the `mmdc` CLI → PNG; charts via any tool → PNG; webtoon/custom
  art → PNG), upload it to the Media Library, and embed only through the core `wp:image`
  block.
- **Banned (plugin/JS-dependent, breaks without the plugin):**
  - Inline `<pre class="mermaid">` / `<div class="mermaid">` (needs a Mermaid plugin)
  - Chart/diagram shortcodes like `[chart]`, `[mermaid]`, `[diagram]`, `[graph]`
  - Plugin-specific Gutenberg blocks (e.g. `wp:plugin-name/chart`)
  - `<script>`-driven embeds (Chart.js, D3, ApexCharts injected into post body)
  - Hotlinked third-party iframes/widgets for diagrams
- Mermaid is allowed **only as a local CLI render step** (`mmdc`) whose output is a static
  PNG. The Mermaid source never ships in the post body.
- Tables (`wp:table`) and infographic boxes (`wp:group`) are core blocks and stay allowed —
  they need no plugin.

### Visual Element Selection Criteria

| Content Type | Recommended Visual Element |
|--------------|---------------------------|
| Post overview / scope | **Mind map** (Mermaid `mindmap`) at the top |
| Process/Workflow | Mermaid flowchart, step diagram |
| Decision/Branching logic | Mermaid flowchart with conditionals |
| Comparison/Selection | Comparison table, quadrant chart, pros/cons cards |
| Architecture/Structure | Mermaid diagram, hierarchy chart, layered stack |
| Data/Statistics/Metrics | **Chart/graph** (`xychart-beta`, `pie`), before/after bars |
| Version/Migration history | Mermaid `timeline` |
| Sequence/Flow | Mermaid sequence diagram |
| Concept Relations | Mermaid graph, relationship diagram |
| Story / pain-point narrative | **Webtoon / illustrated panel** (image-gen → upload) |
| Anything else that fits a visual | Open-ended creative visualization (journey strip, labeled map, cards, custom infographic) — list above is not exhaustive |
| Summary/Key Points | Infographic box, highlight card |

### Mandatory Rules

1. **Maximize infographics — aim for one visual per major section, never fewer than 2 per post**
2. **Prefer showing over telling**: if content fits a diagram/chart/map, render it as an image
3. **Complex concepts must be visualized with diagrams**
4. **Comparisons must be organized in tables or quadrant/comparison cards**
5. **Processes must be expressed as flowcharts**
6. **Vary the visual types** — do not repeat the same chart kind; mix mind map + flow + chart + table
7. **Plugin-independent only** — render every infographic to a static image file and embed via
   the core `wp:image` block; never inline Mermaid, shortcodes, or JS chart libraries
8. **Use summary boxes for TL;DR sections**

### Non-Mermaid & Illustrated Visuals (webtoon, custom infographics)

Mermaid covers diagrams/charts/flow/mindmap/timeline. For **webtoon panels, hand-styled
infographics, or illustrated metaphors**, generate the image (any image-gen tool that
outputs a static PNG/JPG/SVG — no plugin/JS dependency), then upload it and embed as a
`wp:image` block:

```bash
# After producing /tmp/webtoon.png (or any infographic image)
python ~/.claude/skills/wp-blog-post/scripts/upload_media.py \
  --file /tmp/webtoon.png \
  --alt-text "Comic panel: the cache stampede problem"
# Then embed the returned URL via the wp:image block (see Image Handling section)
```

Always give each generated image specific, descriptive **alt text** so the infographic stays
accessible and searchable.

### Mermaid Diagrams → PNG Image (Required Workflow)

**Do NOT use `<!-- wp:html --><pre class="mermaid">` inline.** WordPress does not render Mermaid without a plugin.

**Always render to PNG using `mmdc` and upload:**

```bash
# 1. Write diagram to .mmd file
cat > /tmp/diagram.mmd << 'EOF'
flowchart LR
    A[Start] --> B{Condition}
    B -->|Yes| C[Process1]
    B -->|No| D[Process2]
    C --> E[Complete]
    D --> E
EOF

# 2. Render to PNG (mmdc is available at ~/.nvm/versions/node/*/bin/mmdc)
mmdc -i /tmp/diagram.mmd -o /tmp/diagram.png -w 900 --backgroundColor white

# mmdc renders many infographic types, not only flowcharts. Same command, different header:
#   mindmap        → post scope / topic decomposition
#   xychart-beta   → bar/line charts for metrics (before/after, trends)
#   pie            → proportion / share charts
#   timeline       → version history, migration steps
#   quadrantChart  → 2-axis comparison (e.g. effort vs impact)
#   sequenceDiagram→ API calls, multi-actor flows
# Example mindmap header:
#   mindmap
#     root((Post topic))
#       Background
#       Implementation
#       Results

# 3. Upload to WordPress media library
python ~/.claude/skills/wp-blog-post/scripts/upload_media.py \
  --file /tmp/diagram.png \
  --alt-text "Diagram description"

# 4. Use returned URL in wp:image block (see Image Handling section)
```

**Embed as wp:image block:**

```html
<!-- wp:image {"id":MEDIA_ID,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full">
  <img src="MEDIA_URL" alt="Diagram description" class="wp-image-MEDIA_ID"/>
  <figcaption class="wp-element-caption">Caption text</figcaption>
</figure>
<!-- /wp:image -->
```

### HTML Tables (Basic)

Supported by all WordPress installations.

```html
<!-- wp:table -->
<figure class="wp-block-table">
<table>
  <thead>
    <tr>
      <th>Item</th>
      <th>Description</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Item1</td>
      <td>Description1</td>
      <td>Notes1</td>
    </tr>
  </tbody>
</table>
<figcaption>Table title</figcaption>
</figure>
<!-- /wp:table -->
```

### Infographic Box

Styled box for highlighting key information.

**CRITICAL: All content inside `wp:group` MUST use inner block comments. Raw HTML tags (`<h4>`, `<ul>`, `<p>`) without block comments cause "unexpected content" errors.**

```html
<!-- wp:group {"backgroundColor":"cyan-bluish-gray","className":"info-box"} -->
<div class="wp-block-group info-box has-cyan-bluish-gray-background-color has-background" style="padding: 1.5rem; border-radius: 8px;">
<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">💡 Key Points</h4>
<!-- /wp:heading -->
<!-- wp:list -->
<ul class="wp-block-list">
<!-- wp:list-item -->
<li><strong>Point 1:</strong> Description</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Point 2:</strong> Description</li>
<!-- /wp:list-item -->
</ul>
<!-- /wp:list -->
</div>
<!-- /wp:group -->
```

### Comparison Table Pattern

For A vs B comparisons.

```html
<!-- wp:table {"className":"comparison-table"} -->
<figure class="wp-block-table comparison-table">
<table>
  <thead>
    <tr>
      <th>Comparison</th>
      <th>Option A</th>
      <th>Option B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Pros</td>
      <td>✅ Pro1<br>✅ Pro2</td>
      <td>✅ Pro1<br>✅ Pro2</td>
    </tr>
    <tr>
      <td>Cons</td>
      <td>❌ Con1</td>
      <td>❌ Con1</td>
    </tr>
    <tr>
      <td>Best for</td>
      <td>Use case</td>
      <td>Use case</td>
    </tr>
  </tbody>
</table>
</figure>
<!-- /wp:table -->
```

### Step-by-Step Process Card

Visualize steps in tutorials and guides.

```html
<!-- wp:group {"className":"step-card"} -->
<div class="wp-block-group step-card" style="border-left: 4px solid #0073aa; padding-left: 1rem; margin: 1rem 0;">
<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">📌 Step 1: Environment Setup</h4>
<!-- /wp:heading -->
<!-- wp:paragraph -->
<p>Description...</p>
<!-- /wp:paragraph -->
</div>
<!-- /wp:group -->
```

## 6. Post Templates

### Tech Blog Format

```markdown
# [Title: What was done clearly]

## TL;DR
- 3-5 bullet key points

## Background
Why this work was needed

## Implementation

### Step 1: [First task]
Description and code snippet

### Step 2: [Second task]
Description and code snippet

## Problems and Solutions

### Problem 1: [Description]
- **Cause**:
- **Solution**:

## Results
- What was achieved
- Screenshots/demos (if available)

## Conclusion
Lessons learned, next steps
```

### Tutorial Format

```markdown
# [Title: How to ~]

## What You'll Learn
- Learning objectives list

## Prerequisites
- Required environment/tools

## Step 1: [First step]
Detailed description + code

## Step 2: [Second step]
Detailed description + code

## Complete Code
Full finished code

## Summary
Key takeaways
```

## 7. WordPress Publishing

### Using the Publishing Script

```bash
# Categories: auto-selected from existing or newly created
# Tags: auto-generated from content analysis
python .claude/skills/wp-blog-post/scripts/publish_post.py \
  --title "Building REST API with FastAPI" \
  --content-file /tmp/post_content.html \
  --status draft \
  --categories "Backend,Tutorial" \
  --tags "python,fastapi,rest-api,backend,tutorial"
```

### Options

| Option | Description | Default |
|--------|-------------|---------|
| `--status` | draft / publish | draft |
| `--categories` | Categories (comma-separated, auto-selected) | - |
| `--tags` | Tags (comma-separated, auto-generated) | - |
| `--featured-image` | Featured image path | - |

**Notes**:
- `--categories` auto-selects from existing categories or creates new ones as needed.
- `--tags` are auto-generated through content analysis.
- Manual specification is also available.

## 8. Execution Procedure

1. **Create content file**: Convert markdown to HTML and save to `/tmp/post_content.html`
2. **Generate visual elements (infographic-first)**: Visualize each major section — aim for one infographic per section (minimum 2 per post). Mix types: mind map for scope, flowchart for process, chart/graph for metrics, sequence for flows, webtoon/illustration for narrative. See `Visual Elements Guidelines → Infographic-First Principle`.
3. **Query existing categories**: Fetch category list from WordPress
4. **Auto-select categories**: Select matching existing categories or decide to create new ones
5. **Auto-generate tags**: Extract 5-10 relevant tags from content analysis
6. **Check environment variables**: `WP_SITE_URL`, `WP_USERNAME`, `WP_APP_PASSWORD`
7. **Run AI Slop self-check**: See `Writing Style Rules → AI Slop Prevention → Pre-Publish Self-Check`. 한 항목이라도 실패 시 발행 중단하고 초안 수정.
8. **Execute publishing script**: Save as draft with auto-selected categories and tags
9. **Confirm result**: Provide returned post URL

## Gutenberg Block Rules (CRITICAL)

Violating these rules causes "unexpected or invalid content" errors in the block editor.

### Rule 1: Inner blocks inside wp:group MUST have block comments

Every HTML element inside `<!-- wp:group -->` must be wrapped in its own block comment:

| Element | Required block comment |
|---------|------------------------|
| `<h2>`, `<h3>`, `<h4>` | `<!-- wp:heading {"level":N} -->` |
| `<p>` | `<!-- wp:paragraph -->` |
| `<ul>` | `<!-- wp:list -->` |
| `<ol>` | `<!-- wp:list -->{"ordered":true}` |

### Rule 2: List items require wp:list-item (WordPress 6.0+)

```html
<!-- wp:list -->
<ul class="wp-block-list">
<!-- wp:list-item -->
<li>Item text</li>
<!-- /wp:list-item -->
</ul>
<!-- /wp:list -->
```

### Rule 3: Heading tags require class attribute

```html
<!-- wp:heading {"level":2} -->
<h2 class="wp-block-heading">Title</h2>
<!-- /wp:heading -->
```

### Rule 4: Never use raw HTML inside wp:group

```html
<!-- WRONG - causes block error -->
<!-- wp:group -->
<div class="wp-block-group">
  <h4>Title</h4>     ← raw HTML without block comment
  <ul><li>item</li></ul>
</div>
<!-- /wp:group -->

<!-- CORRECT -->
<!-- wp:group -->
<div class="wp-block-group">
<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Title</h4>
<!-- /wp:heading -->
<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item --><li>item</li><!-- /wp:list-item --></ul>
<!-- /wp:list -->
</div>
<!-- /wp:group -->
```

## Writing Style Rules

### Korean Verb-Ending Periods

- **Korean verb endings must have a period**: When a sentence ends with a verb form (~다, ~하다), it must end with "~다." (with period).
  - Correct: "FastAPI를 사용하여 REST API를 구현했다."
  - Wrong: "FastAPI를 사용하여 REST API를 구현했다"
- Applies to all verb-ending forms: ~했다, ~된다, ~있다, ~없다, ~한다, ~이다, etc.

### AI Slop Prevention (CRITICAL)

단일 패스 생성은 일반 튜토리얼처럼 균질화되기 쉽다. 아래 규칙을 위반한 초안은 발행하지 않는다.

#### 금지 오프닝 (Banned Openings)

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

#### 금지 filler (Banned Filler Phrases)

등장하면 삭제하거나 구체 표현으로 대체한다.

- 수식어: `혁신적인`, `놀라운`, `강력한`, `효율적인`, `최신의`
- 양화: `다양한 ~`, `수많은 ~`, `여러 가지 ~` → 실제 개수·이름으로
- 단정: `매우 중요합니다`, `꼭 필요합니다`, `핵심입니다` → 중요한 이유를 쓰거나 삭제
- 완충: `~라고 할 수 있습니다`, `~라고 볼 수 있습니다` → 근거 제시 또는 단정
- 전환: `자, 이제 ~`, `다음으로 ~`, `마지막으로 ~` → 제목으로 대체

#### 가짜 코드 금지 (No Placeholder Code)

- `doSomething()`, `foo`, `bar`, `...`, `TODO`, `// 생략`, `pass # 구현 예정` 금지
- 코드 블록의 함수·변수는 이 세션에서 실제로 쓴 것과 이름이 일치해야 함
- 불가피한 축약은 `# ... (위와 동일한 import)` 처럼 **축약 의도를 명시**
- 모든 코드 블록에 language tag (` ```python `, ` ```bash ` 등) 필수

#### 구체성 강제 (Specificity Over Generality)

- 구체 수치·버전·에러 메시지·파일 경로를 포함한다.
  - ❌ "성능이 개선되었다"
  - ✅ "p95 응답 시간이 820ms → 190ms로 줄었다"
- 이 세션에서만 얻은 관찰(**insight angle**)을 최소 1건 포함한다.
  - 예: "문서화된 기본 TTL 60초와 달리 내부적으로 600초로 강제된다."
- Stack Overflow 복사 수준의 일반 패턴만 나열하면 실패.

#### 발행 전 Self-Check (MANDATORY)

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

## Code Block Handling

For code highlighting in WordPress:

```html
<!-- Using Prism.js or highlight.js -->
<pre><code class="language-python">
def example():
    pass
</code></pre>

<!-- Gutenberg code block -->
<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>def example():
    pass</code></pre>
<!-- /wp:code -->
```

## Image Handling

Upload local images to WordPress first, then use URL:

```bash
python .claude/skills/wp-blog-post/scripts/upload_media.py \
  --file ./screenshot.png \
  --alt-text "Screenshot description"
```
