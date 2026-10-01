---
name: collab-content-formatter
description: Format tasks, discussions, and comments for collab project management, which uses HTML body fields (never Markdown). Use this skill whenever the user is drafting or writing any content for collab and needs it correctly formatted — the right tags and conventions for spacing, headings, lists, tables, callouts, code, and links. Covers every formatting element and the rules for clean, scannable, professional output.
compatibility: Collab project management platform (HTML body fields only)
---

# Collab Content Formatter

A formatting reference for collab task, discussion, and comment bodies. These fields accept **HTML only** — never Markdown. Reproduce the tags and the spacing convention below exactly; that's what produces clean, scannable output.

This skill covers formatting, not content structure. How to scaffold a task or discussion (objective, acceptance criteria, options tables, etc.) is out of scope.

---

## Core rules

1. **HTML only, no Markdown.** Body fields render HTML. A Markdown `## heading` or `- bullet` shows up as literal text. Always use the tags below.
2. **One blank line between sections and paragraphs — use `<p>&nbsp;</p>`.** This is Collab's own native convention: its editor is TinyMCE, and adding a blank line there inserts exactly this. The non-breaking space is what gives the paragraph **height**. A truly empty `<p></p>` is *not* stripped on save (Collab keeps the tag), but with no content its line box collapses to zero height — so nothing renders and the surrounding blocks butt together into a wall of text. `<p>&nbsp;</p>` carries a full line of height, so the gap actually shows. Bare newlines between block tags don't help either — HTML treats them as insignificant whitespace and renders them as nothing. Put one `<p>&nbsp;</p>` between each section (after a block, before the next heading) and between stacked paragraphs. This is the single most important formatting habit for readability — the example below shows it in place. (On save the `&nbsp;` entity is converted to a literal non-breaking-space character, so in the database you'll see it as `<p> </p>` — that is the same thing and is correct.)
3. **One `<h1>` per item**, used for the title. Use `<h2>` for major sections. Don't use headings as a substitute for bold in body text.
4. **Bold selectively.** Emphasize 1–3 key terms per section. Bolding everything emphasizes nothing.
5. **Tables for 3+ comparable items.** A table scans faster than prose or a long bulleted list. Use lists for simple, non-comparative items.
6. **Title fields are plain text — no HTML entities or tags.** The `name` field of a task, discussion, or task list (and the subtask name) is *not* HTML: Collab shows it as-is, so `&amp;` renders literally as `&amp;`. Write the characters directly — `R&D`, `A < B`, `"quoted"`, `it's`, `—` — never `&amp;`, `&lt;`, `&gt;`, `&quot;`, `&#39;`, `&nbsp;`, `&mdash;`, and never `<strong>` or other tags. The same applies to the text inside `<h1>` in the body: type the title as plain text (a literal `&` is fine; the editor escapes it on save).

---

## Formatting elements

### Text
- **Bold**: `<strong>text</strong>` — key terms, decisions, action items
- *Italic*: `<em>text</em>` — context, alternatives, asides
- Underline: `<span style="text-decoration: underline">text</span>` — rare, urgent attention
- Strikethrough: `<span style="text-decoration: line-through">text</span>` — deprecated/resolved items
- Inline code: `<code>text</code>` — file names, variables, commands, system names

### Headings
- `<h1 class="visual_editor_h1">Title</h1>` — once per item
- `<h2 class="visual_editor_h2">Section</h2>` — major sections

### Spacing
- `<p>&nbsp;</p>` — one blank line. Place between sections and between paragraphs (see Core rule 2).

Never rely on an empty `<p></p>` (or bare newlines between tags) for spacing — an empty paragraph collapses to zero height, so no gap renders even though Collab keeps the tag:

```html
<!-- WRONG: no height-bearing spacer → blocks render with no blank line, sections collapse together -->
<h1 class="visual_editor_h1">Title</h1>
<p>Objective: ...</p>
<h2 class="visual_editor_h2">Konteks</h2>

<!-- RIGHT: <p>&nbsp;</p> carries line height → a real blank line renders between every section -->
<h1 class="visual_editor_h1">Title</h1>
<p>&nbsp;</p>
<p>Objective: ...</p>
<p>&nbsp;</p>
<h2 class="visual_editor_h2">Konteks</h2>
```

### Lists
```html
<ul>
<li>Non-sequential item</li>
<li>Another item</li>
</ul>

<ol>
<li>First step (order matters)</li>
<li>Second step</li>
</ol>
```

### Tables
```html
<table class="visual_editor_table">
<tbody>
<tr>
<td style="width: 33.3%"><strong>Column 1</strong></td>
<td style="width: 33.3%"><strong>Column 2</strong></td>
<td style="width: 33.3%"><strong>Column 3</strong></td>
</tr>
<tr>
<td>Data</td>
<td>Data</td>
<td>Data</td>
</tr>
</tbody>
</table>
```

### Callouts
Same structure for all; swap the second class to set the color/intent:
```html
<aside class="callout-wrapper aside-info">
<div class="callout-content">
<p>Your message</p>
</div>
</aside>
```
| Class | Color | Use for |
|-------|-------|---------|
| `aside-note` | gray | general side note |
| `aside-info` | blue | clarification, helpful detail |
| `aside-success` | green | completed / positive outcome |
| `aside-alert` | red | blocker, warning, critical issue |
| `aside-general` | neutral | generic emphasis |
| `aside-magic` | special | key insight, decision driver |

Use callouts sparingly — one well-placed callout prevents a misunderstanding; five of them are noise.

### Other
- Horizontal rule: `<hr>` — separate major sections (once per item max)
- Blockquote: `<blockquote><p>cited decision or requirement</p></blockquote>` — for quoting others/prior decisions, not your own statements
- Code block: `<pre class="language-bash" data-language="bash"><code>$ command --flag</code></pre>` (swap `language-php`, `language-python`, etc.)
- Link: `<a href="url" target="_blank" rel="noopener noreferrer">text</a>`
- Images: attach via the UI; alt text auto-generates from the filename

---

## Example: formatting in context

Shows headings, bold, inline code, an ordered list, a callout, and the `<p>&nbsp;</p>` spacing rhythm together.

```html
<h1 class="visual_editor_h1">Fix PostgreSQL pg_hba.conf authentication rejection from pod IPs</h1>
<p>&nbsp;</p>
<p><strong>Objective</strong>: Resolve authentication failures when Kubernetes pods connect to PostgreSQL.</p>
<p>&nbsp;</p>
<h2 class="visual_editor_h2">Context</h2>
<p>The Camunda service in staging cannot authenticate to PostgreSQL. Logs show connections from pod CIDR blocks rejected by <code>pg_hba.conf</code>.</p>
<p>&nbsp;</p>
<h2 class="visual_editor_h2">Steps</h2>
<ol>
<li>SSH to the PostgreSQL server</li>
<li>Edit <code>/etc/postgresql/pg_hba.conf</code></li>
<li>Add: <code>host camunda all 10.244.0.0/16 md5</code></li>
<li>Reload: <code>pg_ctl reload</code></li>
</ol>
<p>&nbsp;</p>
<aside class="callout-wrapper aside-info">
<div class="callout-content">
<p><strong>Related</strong>: see task #1234 for Keycloak config if touching auth services.</p>
</div>
</aside>
```

---

## Pre-post checklist

- HTML only — no stray Markdown
- Title (`name`) is plain text — no `&amp;`/`&quot;`/`&#39;`/`&nbsp;` entities or tags; write `&`, quotes, dashes directly
- One `<p>&nbsp;</p>` between every section and stacked paragraph
- Exactly one `<h1>`; `<h2>` for sections
- Bold limited to key terms; callouts only where they matter
- Code wrapped in `<code>` or a code block; links use the full `<a ... rel="noopener noreferrer">` form
- Formatting is purposeful, not decorative; no typos
