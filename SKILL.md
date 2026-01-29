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

Include appropriate visual elements to **enhance readability and comprehension**.

### Visual Element Selection Criteria

| Content Type | Recommended Visual Element |
|--------------|---------------------------|
| Process/Workflow | Mermaid flowchart, step diagram |
| Comparison/Selection | Comparison table, pros/cons table |
| Architecture/Structure | Mermaid diagram, hierarchy chart |
| Data/Statistics | HTML table, summary card |
| Sequence/Flow | Mermaid sequence diagram |
| Concept Relations | Mermaid graph, relationship diagram |
| Summary/Key Points | Infographic box, highlight card |

### Mandatory Rules

1. **Include minimum 2 visual elements per post**
2. **Complex concepts must be visualized with diagrams**
3. **Comparisons must be organized in tables**
4. **Processes must be expressed as flowcharts**
5. **Use summary boxes for TL;DR sections**

### Mermaid Diagrams (Recommended)

Auto-renders with Mermaid plugin in WordPress.

```html
<!-- wp:html -->
<pre class="mermaid">
flowchart LR
    A[Start] --> B{Condition}
    B -->|Yes| C[Process1]
    B -->|No| D[Process2]
    C --> E[Complete]
    D --> E
</pre>
<!-- /wp:html -->
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

```html
<!-- wp:group {"backgroundColor":"cyan-bluish-gray","className":"info-box"} -->
<div class="wp-block-group info-box has-cyan-bluish-gray-background-color has-background">
  <h4>💡 Key Points</h4>
  <ul>
    <li><strong>Point 1:</strong> Description</li>
    <li><strong>Point 2:</strong> Description</li>
  </ul>
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
  <h4>📌 Step 1: Environment Setup</h4>
  <p>Description...</p>
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
2. **Generate visual elements**: Include at least 2 appropriate diagrams, tables, infographics based on content analysis
3. **Query existing categories**: Fetch category list from WordPress
4. **Auto-select categories**: Select matching existing categories or decide to create new ones
5. **Auto-generate tags**: Extract 5-10 relevant tags from content analysis
6. **Check environment variables**: `WP_SITE_URL`, `WP_USERNAME`, `WP_APP_PASSWORD`
7. **Execute publishing script**: Save as draft with auto-selected categories and tags
8. **Confirm result**: Provide returned post URL

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
