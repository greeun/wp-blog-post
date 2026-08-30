# Visual Blocks — Render & Embed Patterns

SKILL.md "5. Visual Elements Guidelines"의 구체 렌더/임베드 패턴을 담는다. 인포그래픽을 실제 이미지로 렌더링하거나 Gutenberg 블록으로 임베드할 때(실행 절차 2단계) 이 파일을 Read한다. 상위 원칙(Infographic-First, Plugin-Independent, Selection Criteria, Mandatory Rules)은 SKILL.md에 그대로 있다.

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
