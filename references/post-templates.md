# Blog Post Templates

## Tech Blog - Implementation Record

```html
<h2>TL;DR</h2>
<ul>
  <li>Key point 1</li>
  <li>Key point 2</li>
  <li>Key point 3</li>
</ul>

<h2>Background</h2>
<p>Explain why this work was needed.</p>

<h2>Implementation</h2>

<h3>Step 1: [Task name]</h3>
<p>Description</p>

<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>def example():
    pass</code></pre>
<!-- /wp:code -->

<h3>Step 2: [Task name]</h3>
<p>Description</p>

<h2>Problems and Solutions</h2>

<h3>Problem: [Description]</h3>
<p><strong>Cause:</strong> Explanation of cause</p>
<p><strong>Solution:</strong> How it was solved</p>

<h2>Results</h2>
<ul>
  <li>Achievement 1</li>
  <li>Achievement 2</li>
</ul>

<h2>Conclusion</h2>
<p>Lessons learned and next steps</p>
```

## Tutorial - Step-by-Step Guide

```html
<h2>What You'll Learn</h2>
<ul>
  <li>Learning objective 1</li>
  <li>Learning objective 2</li>
</ul>

<h2>Prerequisites</h2>
<ul>
  <li>Required environment/tools</li>
</ul>

<h2>Step 1: [Step name]</h2>
<p>Detailed description</p>

<!-- wp:code {"language":"bash"} -->
<pre class="wp-block-code"><code>npm install example</code></pre>
<!-- /wp:code -->

<h2>Step 2: [Step name]</h2>
<p>Detailed description</p>

<h2>Complete Code</h2>

<!-- wp:code {"language":"javascript"} -->
<pre class="wp-block-code"><code>// Complete finished code</code></pre>
<!-- /wp:code -->

<h2>Summary</h2>
<p>Key takeaways</p>
```

## Project Update - Progress Report

```html
<h2>Update Summary</h2>
<ul>
  <li>Completed task 1</li>
  <li>Completed task 2</li>
</ul>

<h2>Major Changes</h2>

<h3>Feature 1: [Feature name]</h3>
<p>Description and screenshot</p>

<h3>Feature 2: [Feature name]</h3>
<p>Description and screenshot</p>

<h2>Next Steps</h2>
<ul>
  <li>Planned task 1</li>
  <li>Planned task 2</li>
</ul>
```

## Gutenberg Block Reference

### Code Block
```html
<!-- wp:code {"language":"python"} -->
<pre class="wp-block-code"><code>Code content</code></pre>
<!-- /wp:code -->
```

### Image
```html
<!-- wp:image {"id":123,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large">
  <img src="image-url" alt="alt text"/>
  <figcaption>Caption</figcaption>
</figure>
<!-- /wp:image -->
```

### Blockquote
```html
<!-- wp:quote -->
<blockquote class="wp-block-quote">
  <p>Quote content</p>
  <cite>Source</cite>
</blockquote>
<!-- /wp:quote -->
```

### Callout Box
```html
<!-- wp:paragraph {"backgroundColor":"pale-cyan-blue"} -->
<p class="has-pale-cyan-blue-background-color has-background">
  ℹ️ Informational message
</p>
<!-- /wp:paragraph -->
```

## Visual Element Templates

### Mermaid Flowchart

For processes, workflows, decision flows:

```html
<!-- wp:html -->
<pre class="mermaid">
flowchart TD
    A[🚀 Start] --> B{Check Condition}
    B -->|Condition met| C[Execute Process]
    B -->|Condition not met| D[Alternative Process]
    C --> E[✅ Complete]
    D --> E

    style A fill:#e1f5fe
    style E fill:#c8e6c9
</pre>
<!-- /wp:html -->
```

### Mermaid Sequence Diagram

For API calls, component communication, event flows:

```html
<!-- wp:html -->
<pre class="mermaid">
sequenceDiagram
    participant C as Client
    participant S as Server
    participant D as Database

    C->>S: Send Request
    S->>D: Query Data
    D-->>S: Return Result
    S-->>C: Send Response
</pre>
<!-- /wp:html -->
```

### Mermaid Architecture Diagram

For system structure, component relationships:

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

### Comparison Table (A vs B)

For technology selection, library comparison:

```html
<!-- wp:table {"className":"is-style-stripes"} -->
<figure class="wp-block-table is-style-stripes">
<table>
  <thead>
    <tr>
      <th>Comparison</th>
      <th>🅰️ Option A</th>
      <th>🅱️ Option B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Pros</strong></td>
      <td>✅ Fast performance<br>✅ Easy learning curve</td>
      <td>✅ Rich ecosystem<br>✅ Strong type support</td>
    </tr>
    <tr>
      <td><strong>Cons</strong></td>
      <td>❌ Limited features<br>❌ Small community</td>
      <td>❌ Complex setup<br>❌ Steep learning curve</td>
    </tr>
    <tr>
      <td><strong>Best for</strong></td>
      <td>Small projects</td>
      <td>Large projects</td>
    </tr>
  </tbody>
</table>
<figcaption>Comparison of Option A and Option B</figcaption>
</figure>
<!-- /wp:table -->
```

### Feature Summary Table

For implemented features, API endpoint documentation:

```html
<!-- wp:table -->
<figure class="wp-block-table">
<table>
  <thead>
    <tr>
      <th>Feature</th>
      <th>Description</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>🔐 Authentication</td>
      <td>JWT-based user authentication</td>
      <td>✅ Complete</td>
    </tr>
    <tr>
      <td>📝 CRUD</td>
      <td>Basic data management</td>
      <td>✅ Complete</td>
    </tr>
    <tr>
      <td>🔍 Search</td>
      <td>Full-text search functionality</td>
      <td>🚧 In Progress</td>
    </tr>
  </tbody>
</table>
<figcaption>Feature Implementation Status</figcaption>
</figure>
<!-- /wp:table -->
```

### Infographic - Key Summary Box

For TL;DR, key points emphasis:

```html
<!-- wp:group {"backgroundColor":"cyan-bluish-gray","className":"summary-box"} -->
<div class="wp-block-group summary-box has-cyan-bluish-gray-background-color has-background" style="padding: 1.5rem; border-radius: 8px;">
  <h4 style="margin-top: 0;">💡 Key Summary</h4>
  <ul style="margin-bottom: 0;">
    <li><strong>Problem:</strong> Limitations of existing approach</li>
    <li><strong>Solution:</strong> New approach taken</li>
    <li><strong>Result:</strong> Achievements accomplished</li>
  </ul>
</div>
<!-- /wp:group -->
```

### Infographic - Warning Box

For important cautions, warning messages:

```html
<!-- wp:group {"backgroundColor":"vivid-red","className":"warning-box"} -->
<div class="wp-block-group warning-box" style="background: #fff3cd; border-left: 4px solid #ffc107; padding: 1rem; margin: 1rem 0;">
  <h4 style="margin-top: 0; color: #856404;">⚠️ Caution</h4>
  <p style="margin-bottom: 0; color: #856404;">This action cannot be undone. Backup recommended before execution.</p>
</div>
<!-- /wp:group -->
```

### Infographic - Tip Box

For useful tips, additional information:

```html
<!-- wp:group {"className":"tip-box"} -->
<div class="wp-block-group tip-box" style="background: #d4edda; border-left: 4px solid #28a745; padding: 1rem; margin: 1rem 0;">
  <h4 style="margin-top: 0; color: #155724;">💚 Pro Tip</h4>
  <p style="margin-bottom: 0; color: #155724;">Managing environment variables with .env files provides both security and convenience.</p>
</div>
<!-- /wp:group -->
```

### Step-by-Step Process Card

For visualizing tutorial and guide steps:

```html
<!-- wp:group {"className":"step-cards"} -->
<div class="wp-block-group step-cards">

  <!-- Step 1 -->
  <div style="border-left: 4px solid #0073aa; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #0073aa;">📌 Step 1: Project Initialization</h4>
    <p style="margin: 0.5rem 0 0;">Create a new project with npm init.</p>
  </div>

  <!-- Step 2 -->
  <div style="border-left: 4px solid #0073aa; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #0073aa;">📌 Step 2: Install Dependencies</h4>
    <p style="margin: 0.5rem 0 0;">Install required packages.</p>
  </div>

  <!-- Step 3 -->
  <div style="border-left: 4px solid #28a745; padding: 0.5rem 1rem; margin: 1rem 0; background: #f8f9fa;">
    <h4 style="margin: 0; color: #28a745;">✅ Step 3: Complete</h4>
    <p style="margin: 0.5rem 0 0;">Setup is complete.</p>
  </div>

</div>
<!-- /wp:group -->
```

### Statistics/Metrics Highlight

For performance improvements, result metrics emphasis:

```html
<!-- wp:columns -->
<div class="wp-block-columns">

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #e3f2fd; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #1976d2; margin: 0;">50%</p>
    <p style="margin: 0; color: #666;">Performance Improvement</p>
  </div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #e8f5e9; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #388e3c; margin: 0;">3x</p>
    <p style="margin: 0; color: #666;">Processing Speed</p>
  </div>
  <!-- /wp:column -->

  <!-- wp:column -->
  <div class="wp-block-column" style="text-align: center; padding: 1rem; background: #fff3e0; border-radius: 8px;">
    <p style="font-size: 2.5rem; font-weight: bold; color: #f57c00; margin: 0;">99.9%</p>
    <p style="margin: 0; color: #666;">Availability</p>
  </div>
  <!-- /wp:column -->

</div>
<!-- /wp:columns -->
```
