---
name: wp-blog-post
description: |
  Convert work session content into WordPress blog posts and publish.
  Use when: "write blog", "publish post", "post to wordpress", "tech blog", "write tutorial",
  "summarize session as blog", "turn work into blog post"
version: 1.0.0
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

### 렌더 & 임베드 패턴 (구체 블록)

구체 렌더/임베드 패턴 — webtoon/커스텀 인포그래픽 생성, Mermaid → PNG 워크플로우(`mmdc`), HTML 표, 인포그래픽 박스, 비교 표, Step-by-Step 프로세스 카드 — 은 [references/visual-blocks.md](references/visual-blocks.md)에 있다. 인포그래픽을 실제 이미지로 렌더링하거나 Gutenberg 블록으로 임베드할 때(실행 절차 2단계) 이 파일을 Read한다. **Mermaid는 로컬 `mmdc` CLI 렌더 후 정적 PNG로만 임베드**하며 본문에 Mermaid 소스/샷코드/JS 차트를 넣지 않는다.

## 6. Post Templates

바로 붙여 쓸 수 있는 HTML 포맷 전체 템플릿(TL;DR·구현 기록 등)은 [references/post-templates.md](references/post-templates.md)(영문), [references/post-templates.ko.md](references/post-templates.ko.md)(국문)에 있다. 아래는 마크다운 골격 예시다.

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
| `--featured-media` | Featured image media ID (integer, from an already-uploaded WordPress media item) | - |

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

Violating these rules causes "unexpected or invalid content" errors in the block editor. 4개 규칙(wp:group 내부 블록 주석 필수, wp:list-item 필수, heading class 속성 필수, wp:group 내 raw HTML 금지)의 상세 예시는 [references/gutenberg-rules.md](references/gutenberg-rules.md)에 있다. 콘텐츠 HTML을 조립할 때 이 파일을 Read한다.

## Writing Style Rules

### Korean Verb-Ending Periods

- **Korean verb endings must have a period**: When a sentence ends with a verb form (~다, ~하다), it must end with "~다." (with period).
  - Correct: "FastAPI를 사용하여 REST API를 구현했다."
  - Wrong: "FastAPI를 사용하여 REST API를 구현했다"
- Applies to all verb-ending forms: ~했다, ~된다, ~있다, ~없다, ~한다, ~이다, etc.

### AI Slop Prevention (CRITICAL)

단일 패스 생성은 일반 튜토리얼처럼 균질화되기 쉽다. 금지 오프닝, 금지 filler, 가짜 코드 금지, 구체성 강제, 그리고 **발행 전 Self-Check (MANDATORY)**의 전체 목록과 검사용 grep은 [references/writing-style.md](references/writing-style.md)에 있다. 초안 작성 시와 발행 직전(실행 절차 7단계)에 이 파일을 Read하고, 한 항목이라도 실패하면 발행을 중단한다.

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
