# Gutenberg Block Rules (CRITICAL)

SKILL.md "Gutenberg Block Rules"의 상세. 콘텐츠 HTML을 조립하거나 `wp:group`/`wp:list`/`wp:heading` 블록을 작성할 때 이 파일을 Read한다. 이 규칙 위반은 블록 에디터에서 "unexpected or invalid content" 에러를 일으킨다.

Violating these rules causes "unexpected or invalid content" errors in the block editor.

## Rule 1: Inner blocks inside wp:group MUST have block comments

Every HTML element inside `<!-- wp:group -->` must be wrapped in its own block comment:

| Element | Required block comment |
|---------|------------------------|
| `<h2>`, `<h3>`, `<h4>` | `<!-- wp:heading {"level":N} -->` |
| `<p>` | `<!-- wp:paragraph -->` |
| `<ul>` | `<!-- wp:list -->` |
| `<ol>` | `<!-- wp:list -->{"ordered":true}` |

## Rule 2: List items require wp:list-item (WordPress 6.0+)

```html
<!-- wp:list -->
<ul class="wp-block-list">
<!-- wp:list-item -->
<li>Item text</li>
<!-- /wp:list-item -->
</ul>
<!-- /wp:list -->
```

## Rule 3: Heading tags require class attribute

```html
<!-- wp:heading {"level":2} -->
<h2 class="wp-block-heading">Title</h2>
<!-- /wp:heading -->
```

## Rule 4: Never use raw HTML inside wp:group

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
