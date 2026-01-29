# WordPress Blog Post Skill

A Claude Code skill that automatically converts work session content into WordPress blog posts and publishes them.

**[한국어 문서 (Korean)](./README.ko.md)**

## Installation

### 1. Environment Variables Setup

Add to `~/.zshrc` or `~/.bashrc`:

```bash
export WP_SITE_URL="https://your-site.com"
export WP_USERNAME="your-username"
export WP_APP_PASSWORD="xxxx xxxx xxxx xxxx xxxx xxxx"
```

After setup, restart terminal or run `source ~/.zshrc`.

### 2. Creating Application Password

1. Log in to WordPress Admin
2. Navigate to **Users** → **Profile**
3. Scroll to **Application Passwords** section at bottom
4. Enter name (e.g., "Claude Code") → **Add New Application Password**
5. Set generated password as `WP_APP_PASSWORD`

> Application Password is a built-in feature in WordPress 5.6+. Earlier versions require a plugin.

## Usage

### Invoking the Skill in Claude Code

The skill automatically activates with requests like:

```
Write a blog post
Post to wordpress
Write a tech blog
Create a tutorial
Summarize session as blog
```

### Direct Script Usage

#### Publishing a Post

```bash
# Convert markdown file to HTML
python .claude/skills/wp-blog-post/scripts/md_to_html.py \
  --input post.md \
  --output /tmp/post.html

# Publish to WordPress (as draft)
python .claude/skills/wp-blog-post/scripts/publish_post.py \
  --title "Post Title" \
  --content-file /tmp/post.html \
  --status draft \
  --categories "Development,Tech" \
  --tags "python,automation"
```

#### Options

| Option | Description | Example |
|--------|-------------|---------|
| `--title` | Post title (required) | `"Using Claude Code"` |
| `--content` | Direct HTML content | `"<p>Content</p>"` |
| `--content-file` | HTML file path | `/tmp/post.html` |
| `--status` | Publish status | `draft`, `publish`, `pending`, `private` |
| `--categories` | Categories (comma-separated) | `"Development,Tech"` |
| `--tags` | Tags (comma-separated) | `"python,claude,ai"` |
| `--featured-media` | Featured image ID | `123` |

#### Image Upload

```bash
python .claude/skills/wp-blog-post/scripts/upload_media.py \
  --file ./screenshot.png \
  --alt-text "Screenshot description" \
  --caption "Image caption"
```

Output:
```json
{
  "success": true,
  "id": 456,
  "url": "https://your-site.com/wp-content/uploads/2024/01/screenshot.png"
}
```

The returned `id` can be used with `--featured-media`.

## Visual Elements (Auto-generated)

Visual elements are **automatically included** based on content when running the skill.

### Supported Visual Elements

| Element | Purpose | Example |
|---------|---------|---------|
| **Mermaid Flowchart** | Process, workflow | Deployment pipeline, decision flow |
| **Mermaid Sequence** | API communication, event flow | Client-server request/response |
| **Mermaid Architecture** | System structure | Component relationship diagram |
| **Comparison Table** | A vs B comparison | Library selection, tech comparison |
| **Feature Summary Table** | Implementation status | API endpoints, feature list |
| **Infographic Box** | Key summary, tips, warnings | TL;DR, Pro Tip, Caution |
| **Statistics Highlight** | Performance metrics | 50% improvement, 3x speed |
| **Step Cards** | Tutorial steps | Step 1, 2, 3... |

### Application Rules

- **Minimum 2 visual elements** per post
- Complex concepts → **Diagram** required
- Comparisons → **Comparison table** required
- Process descriptions → **Flowchart** required

### Example: Mermaid Flowchart

```html
<!-- wp:html -->
<pre class="mermaid">
flowchart LR
    A[Request] --> B{Auth Check}
    B -->|Success| C[Process]
    B -->|Fail| D[Return Error]
    C --> E[Response]
</pre>
<!-- /wp:html -->
```

### Example: Comparison Table

```html
<!-- wp:table -->
<figure class="wp-block-table">
<table>
  <thead>
    <tr><th>Item</th><th>Option A</th><th>Option B</th></tr>
  </thead>
  <tbody>
    <tr><td>Pros</td><td>✅ Fast</td><td>✅ Stable</td></tr>
    <tr><td>Cons</td><td>❌ Complex</td><td>❌ Slow</td></tr>
  </tbody>
</table>
</figure>
<!-- /wp:table -->
```

### Example: Infographic Box

```html
<!-- wp:group {"backgroundColor":"cyan-bluish-gray"} -->
<div class="wp-block-group has-cyan-bluish-gray-background-color has-background" style="padding: 1.5rem;">
  <h4>💡 Key Summary</h4>
  <ul>
    <li><strong>Problem:</strong> Limitations of existing approach</li>
    <li><strong>Solution:</strong> New approach</li>
    <li><strong>Result:</strong> 50% performance improvement</li>
  </ul>
</div>
<!-- /wp:group -->
```

> **For Mermaid**: WordPress Mermaid plugin required (e.g., WP Mermaid)

## Post Templates

### Tech Blog

```markdown
# Title

## TL;DR
💡 Key summary infographic box

## Background
Why this work was needed

## Architecture
📊 Mermaid diagram for structure explanation

## Implementation

### Step 1: Task name
Description and code

### Step 2: Task name
Description and code

## Problems and Solutions

### Problem: Description
- **Cause**:
- **Solution**:

## Results
📈 Statistics highlight or feature summary table

## Conclusion
Lessons learned
```

### Tutorial

```markdown
# How to ~

## What You'll Learn
💡 Learning objectives infographic box

## Prerequisites
📋 Requirements table

## Overall Flow
📊 Mermaid flowchart visualizing steps

## Step 1: Step name
📌 Step card + description + code

## Step 2: Step name
📌 Step card + description + code

## Complete Code
Finished code

## Summary
💡 Key summary box
```

## File Structure

```
wp-blog-post/
├── SKILL.md                 # Skill definition (for Claude Code)
├── README.md                # This document
├── README.ko.md             # Korean documentation
├── scripts/
│   ├── publish_post.py      # Post publishing API
│   ├── upload_media.py      # Media upload API
│   └── md_to_html.py        # Markdown→HTML conversion
└── references/
    ├── post-templates.md    # Gutenberg block templates
    └── post-templates.ko.md # Korean templates
```

## Troubleshooting

### "Missing environment variables" Error

Environment variables not set:
```bash
echo $WP_SITE_URL
echo $WP_USERNAME
echo $WP_APP_PASSWORD
```

If empty, set the environment variables again.

### "401 Unauthorized" Error

- Verify Application Password is correct
- Check if REST API is enabled in WordPress
- Verify user has post creation permissions

### "Could not create category" Warning

May lack category creation permission. Use existing category names or request permissions from admin.

### REST API Disabled

Some security plugins block REST API:
- Check Wordfence, iThemes Security settings
- Allow access to `/wp-json/wp/v2/posts` endpoint

## Security Notes

- Keep Application Password secure
- Recommend setting `.bashrc`/`.zshrc` file permissions to `600`
- Consider separate credential file instead of environment variables in shared environments
