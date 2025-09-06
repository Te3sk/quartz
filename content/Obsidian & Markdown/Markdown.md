---
title: Obsidian & Markdown
date: 2025-09-06
tags:
  - markdown
  - doc
category: Obsidian & Markdown
status: bozza
author: Te3sk
description: "Introduction to markdown language: how to use it, syntax overview, cheat sheet and tips"
---
# Introduction
**Markdown** is a lightweight markup language designed to format plain text using simple, human-readable syntax.  
It allows you to create rich text documents (with headings, lists, links, images, and more) without relying on complex editors or proprietary formats.  

Markdown is widely used because it is:
- **Simple** → easy to learn and read, even without rendering.  
- **Portable** → works across different platforms, tools, and workflows.  
- **Compatible** → supported by most documentation systems, GitHub repositories, static site generators, and note-taking apps like Obsidian.  

In practice, Markdown is used for:
- **Documentation** (README files, wikis, technical guides).  
- **Note-taking** (personal or team vaults in Obsidian).  
- **Publishing** (blogs, static websites, or knowledge bases).  
- **Collaboration** (shared files in GitHub or other version control systems).  

Its strength lies in the fact that the **raw text is always readable**, while still being easily converted into HTML, PDF, or other formats when needed.
# Basic Syntax Overview
Markdown provides a simple set of formatting rules that can be combined to structure and style text. Below are the most commonly used elements.
## Headings
Use `#` symbols to define headings. The number of `#` symbols indicates the heading level (from 1 to 6).
```markdown
# Heading 1
## Heading 2
### Heading 3
```
## Paragraphs & Line Breaks
A new paragraph is created by leaving a blank line between blocks of text.  
Line breaks can be forced by adding two spaces at the end of a line.
## Bold & Italic
- Italic: `*italic*` or `_italic_` → _italic_
- Bold: `**bold**` or `__bold__` → **bold**
- Bold + Italic: `***text***` → _**text**_
## Blockquotes
Prefix a line with `>` to create a blockquote.
```markdown
> This is a blockquote.
```
## Lists
You can make unordered lists using `-`, `*` or `+`:
```markdown
- Item 1
- Item 2
- Item 3
```
Or you cane make ordered lists using numbers following by a dot `.`
```markdown
1. Item 1
2. Item 2
3. Item 3
```
## Links
```Markdown
[Digital-On Tech Vault](https://te3sk.github.io/quartz/)
```
**Output:** [Digital-On Tech Vault](https://te3sk.github.io/quartz/)
## Images
Images use the same syntax as links, with an exclamation mark `!` at the beginning.
```Markdown
![Alt text](https://example.com/image.png)
```
## Code
- **Inline code**: wrap text in backticks (`` ` ``).  
    Example: Use the `cd` command to change directory.    
- **Code blocks**: use triple backticks. Optionally specify a language for syntax highlighting.
```javascript
console.log("Hello, world!");
```
## Extended Syntax (Optional but Useful)
### Tables
```Markdown
  | Column 1 | Column 2 |
  |----------|----------|
  | Value A  | Value B  |
  | Value C  | Value D  |
```
Renders as:

| Column 1 | Column 2 |
|----------|----------|
| Value A  | Value B  |
| Value C  | Value D  |

### Task Lists
```Markdown
- [ ] Incomplete task
- [x] Completed task
```
Renders as:
- [ ] Incomplete task
- [x] Completed task
### Horizontal Rule
```Markdown
---
```
renders as:

---
### Escaping Characters
Use backslash `\` before a character to display it literally.
```Markdown
\*Not italicized\*
```
renders as: \*Not italicized\*
# Tips & Best Practices
To make the most out of Markdown, keep in mind these practical guidelines:
- **Keep it readable**  
  Your raw Markdown text should still make sense without rendering.  
  Example: prefer proper headings and spacing instead of forcing styles.
- **Use headings consistently**  
  Start from `#` for the document title, then use `##`, `###`, etc. in order.  
  Don’t skip levels just to adjust text size.
- **Prefer semantic formatting**  
  Use bold for emphasis, italic for nuance, and headings for structure — not just for style.
- **Keep code blocks clean**  
  Always specify a language (e.g., ` ```js `) for syntax highlighting.  
  Avoid mixing code and explanations in the same block.
- **Limit line length**  
  Use shorter lines (70–100 characters) to make diffs easier to read in version control.
- **Stay consistent**  
  Agree on a common style across the team (e.g., heading capitalization, list markers).  
  Consistency improves collaboration and readability.
- **Use preview mode**  
  In Obsidian, VS Code, or GitHub, check the rendered output to confirm formatting works as expected.