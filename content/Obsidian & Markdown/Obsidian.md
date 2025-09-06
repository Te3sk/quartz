---
title: Work with obsidian
date: 2025-09-06
tags:
  - obsidian
  - markdown
  - doc
  - internal-tools
category: Obsidian & Markdown
status: in_corso
author: Te3sk
description: This document explains how to work effectively with **Obsidian**, focusing on the key features it adds on top of Markdown and the core concepts of vaults, notes, and folders that structure a knowledge base.
---
[Obsidian Documentation](https://help.obsidian.md/)
# Introduction
**Obsidian** is a knowledge management and note-taking application built on top of plain Markdown files.  
While Markdown provides the **foundation** — a simple, portable syntax for creating and formatting text — Obsidian enhances it with a wide range of **extra features** that transform a flat collection of notes into a connected, interactive knowledge base.

With Obsidian, notes are not just static documents but part of a dynamic system that supports linking, tagging, visualization, and customization.  
This makes it especially valuable for teams and organizations that need both the **simplicity of Markdown** and the **power of advanced tools** to organize, navigate, and grow their shared documentation.
# Vaults, Notes, and Folders
## Vaults
[Obsidian Doc - Create a Vault](https://help.obsidian.md/vault)
In Obsidian, a **vault** is the root folder that contains all your notes, attachments, and configuration files.  
Every note you create or link inside Obsidian belongs to a specific vault, and each vault is completely **isolated** from the others — meaning plugins, settings, and links do not cross over between vaults.

Most users and teams prefer to maintain **a single vault** for all their documentation, as this allows backlinks, tags, and the graph view to work across the entire knowledge base.  
However, creating multiple vaults can make sense in certain cases, such as:
- Keeping **personal notes** separate from **company documentation**.  
- Managing **sensitive or confidential projects** that require isolation.  
- Maintaining a **test vault** for experimenting with plugins or settings without affecting production notes.

As a general guideline: use **one main vault** whenever possible, and create additional vaults only when there is a strong reason for separation.
## Folders
In Obsidian, **folders** provide a traditional tree structure to organize notes within a vault.  
They allow you to group related notes together, making navigation more intuitive for users who prefer a hierarchical layout.

However, Obsidian also offers **tags** and **internal links**, which can often replace or complement folder structures. Tags let you classify notes across different contexts, while links create meaningful relationships that don’t depend on file location.

**Best practices:**
- Use folders for **broad categories** (e.g., Projects, Processes, Resources).  
- Rely on tags and links for **cross-cutting themes** (e.g., `#todo`, `#meeting`, `#reference`).  
- Avoid deep folder nesting, which can make notes harder to find and maintain.  
- Keep the structure **flexible**, allowing knowledge to grow organically rather than forcing rigid hierarchies.

In short, folders should serve as a **light organizational layer**, while the real power of Obsidian comes from the **network of links and tags** that connect your notes.
## Notes
In Obsidian, every note is a simple **Markdown file (`.md`)** stored inside your vault.  
Notes are the fundamental building blocks of the knowledge base: each one represents a single unit of information that can be connected to others through links, tags, and references.

There are different ways to structure notes depending on their purpose:
- **Atomic notes** → short and focused, capturing one concept, idea, or piece of information. They make linking and reuse easier.  
- **Hub notes** → larger and more structured, designed to summarize, group, or provide an overview of related atomic notes.  

A healthy vault usually contains a **mix of atomic and hub notes**: the first ensures precision and granularity, while the second provides context and navigation.
# Key Features Added by Obsidian
## Internal Links
Obsidian uses **internal links** to connect notes within the same vault.  
The basic syntax is:  
- `[[Note Title]]` → creates a link to another note. If the note does not exist yet, Obsidian will create it automatically when you follow the link.  
### Variants of Internal Links
- **Link to a specific header or section**:  
  `[[Note Title#Header]]` → jumps directly to the specified heading inside the note.  
- **Custom link text**:  
  `[[Link Target|Displayed Text]]` → links to the note but shows a custom label instead of the note’s name.  
### Difference from External Links
This is different from **external links**, which point to resources outside the vault (e.g., `[OpenAI](https://openai.com)`). Internal links ensure that your knowledge base stays interconnected and navigable without leaving Obsidian.
### Connected Knowledge
The true power of internal links lies in the concept of **connected knowledge**: instead of storing information in isolated documents, you build a network of related notes. This allows ideas to be discovered, revisited, and combined in new ways, making the vault more than just a collection of files — it becomes a living system of knowledge.
## Backlinks
A **backlink** in Obsidian is an automatically generated reference that shows all the notes linking to the current note.  
For example, if three different notes contain `[[Project Plan]]`, opening the *Project Plan* note will display a list of those references in the backlinks panel.

Backlinks enable **bidirectional navigation**: instead of only knowing where a link points, you can also see *who is pointing back*.  
This provides valuable **context** — helping you discover connections you may have forgotten, identify clusters of related knowledge, and move fluidly through your vault without relying solely on folder hierarchy.

In practice, backlinks turn isolated notes into a **network of ideas**, making it easier to track dependencies, relationships, and recurring concepts across your documentation.
## Tags
In Obsidian, **tags** are created by adding a `#` followed by a keyword (e.g., `#meeting`, `#todo`).  
Unlike internal links (`[[Note Title]]`), which connect specific notes together, tags are used to **classify and group notes across different contexts**.

The main advantage of tags is that they allow for **transversal organization**: you can filter, search, and collect notes that share the same tag, even if they live in different folders or cover unrelated topics.  
For example, tagging notes with `#urgent` provides a quick way to retrieve all tasks marked as high priority, regardless of where they are stored.

In short, links create **relationships** between specific pieces of knowledge, while tags provide **categories** that cut across the entire vault.
## Graph View
The **Graph View** in Obsidian provides a visual representation of your vault as a network of connected [[#Notes|notes]].  
Each note appears as a node, and the connections between them are drawn from **[[#Internal Links|internal links]]** (`[[Note Title]]`) and **[[#Backlinks|backlinks]]**, showing how ideas relate to one another.  
[[#Tags]] also appear in the graph, grouping notes by shared classifications and offering another layer of structure.

Graph View is especially useful for exploring relationships between concepts that may not be obvious through folders alone.  
It allows you to discover clusters of knowledge, identify isolated notes that need better linking, and create a **knowledge map** that grows organically as your vault expands.

By combining **internal links for relationships**, **backlinks for context**, and **tags for categories**, the Graph View turns your vault into an interactive map where you can navigate both broad themes and detailed connections.

# Settings & Customization
## Workspace Layout
[Obsidian Doc - Workspace](https://help.obsidian.md/workspace)

Obsidian’s **workspace layout** defines how your interface is structured: it includes the arrangement of panes, sidebars, the ribbon, tab groups, and the status bar :contentReference. You can customize and arrange your workspace with:
- **Sidebars**  
  The app features both **left and right sidebars**. On desktop, the left sidebar includes the Ribbon (main menu icons), while both sidebars can be expanded or collapsed to focus on your content :contentReference.
- **Tab Groups & Panes**  
  You can split the central area into **multiple panes**, arranged vertically or horizontally, each containing one or more **tabs** :contentReference. This makes multitasking easy—such as taking notes while viewing other documents.
- **Ribbon & Status Bar**  
  The **Ribbon** (a vertical toolbar) provides quick access to common commands. The **status bar** at the bottom right shows useful info like sync status or active modes :contentReference.
- **Saving & Switching Workspaces**  
  Obsidian’s **Workspaces** (a core plugin) let you save this entire layout—including sidebar visibility, open tabs, and pane arrangement—as a named configuration. You can save, load, or delete workspaces via the command palette or ribbon.
## Appearance
[Obsidian Doc - Appearance](https://help.obsidian.md/appearance)
The **Appearance** settings in Obsidian allow you to fully customize the look and feel of your workspace.  
You can start by choosing the **Base color scheme** (light or dark mode) and an **Accent color** to adjust the main highlight across the interface. The **Themes** menu lets you manage installed themes or browse community-created ones, giving you complete control over the overall design.

Obsidian also provides fine-grained **font customization**:  
- *Interface font* → sets the font for menus, panels, and sidebars.  
- *Text font* → used in the main editing and reading views.  
- *Monospace font* → applied to code blocks and frontmatter.  
You can adjust the **font size** globally, or use quick font size scaling (e.g., `Ctrl + Scroll`) for temporary changes.

In the **Interface section**, options such as showing inline titles, displaying the tab bar, or customizing the ribbon menu let you tailor how information is displayed.  
Under **Advanced settings**, you can control the **zoom level**, enable or disable **native menus**, change the **window frame style**, or even set a **custom app icon**. Features like **translucent window effects** and **hardware acceleration** (GPU rendering) further enhance performance and visual polish.

Finally, Obsidian supports **CSS snippets**, small custom style sheets you can add to fine-tune appearance beyond themes. This makes it possible to create a consistent, branded look across the company vault while still allowing individual flexibility when needed.
## Core Plugins
**Core plugins** are built-in features that extend Obsidian beyond plain Markdown editing.  
They can be enabled or disabled individually from the settings, allowing you to tailor the app to your workflow without installing third-party extensions.  
While some plugins are designed for convenience, others fundamentally shape how you interact with your notes, turning Obsidian into a flexible knowledge management system.

Among the most important core plugins are:
- **Backlinks** → Creates automatic bidirectional links, showing all notes that reference the current one. This is essential for building a connected knowledge graph.  
- **Graph View** → Displays a visual map of all your notes and their relationships, helping you discover clusters of knowledge.  
- **Daily Notes** → Generates a new note for each day, often used for journaling, logging tasks, or quick capture.  
- **Templates** → Lets you insert predefined text or structures into notes, saving time and maintaining consistency across documentation.  
- **Page Preview** → Shows a preview of linked notes when you hover over links, improving navigation without leaving your current context.  
- **Command Palette** → Provides quick access to any command via keyboard shortcuts, making it easier to work efficiently.  
- **Sync & File Recovery** (if enabled) → Help manage note backups, version history, and synchronization across devices.

These plugins form the **foundation of most Obsidian workflows**, offering a balance between structure (like backlinks and templates) and efficiency (like command palette and page previews). By activating only the features you need, you can keep the interface clean while still benefiting from Obsidian’s most powerful capabilities.
## Community Plugins

**Community plugins** are extensions developed by the Obsidian user community.  
They expand the app’s capabilities far beyond what is offered by the built-in core plugins, covering use cases like project management, advanced data visualization, or workflow automation.  
Unlike core plugins, they are optional and must be installed manually, which gives users great flexibility while also requiring some caution to ensure security and stability.
### How to Install and Use Community Plugins
1. **Enable third-party plugins**  
   - Go to **Settings → Community Plugins** and toggle the option to allow third-party extensions.  
   - This unlocks the Obsidian community marketplace.
1. **Browse and Install**  
   - From the Community Plugins menu, open the marketplace to search and browse available plugins.  
   - When you find one that suits your needs, click **Install**, then **Enable** to activate it in your vault.
1. **Configure Settings**  
   - Most community plugins add a section in the **Settings** panel.  
   - Here you can adjust their behavior, set defaults, or customize how they integrate into your workspace.
1. **Assign Hotkeys**  
   - After installation, new commands provided by the plugin appear in the **Hotkeys** menu.  
   - You can search for the plugin name, then assign or change shortcuts to match your workflow.  
   - This makes it easy to trigger plugin actions without leaving your keyboard.

By carefully selecting and configuring community plugins, you can tailor Obsidian to your team’s specific workflows while still keeping the vault consistent and manageable. The key is to balance flexibility with discipline: install only what is truly useful, and make sure hotkeys and settings are documented for shared team usage.
### Security Considerations
While community plugins greatly enhance Obsidian’s functionality, they are **third-party extensions** and are not officially audited by the Obsidian team.  
For this reason, it’s important to follow some best practices:
- **Install only from trusted sources** → Prefer plugins available in the official Obsidian community marketplace. Avoid downloading or enabling plugins from unverified repositories.  
- **Keep plugins updated** → Developers often release updates that fix bugs or patch vulnerabilities. Regularly check for updates in the Community Plugins menu.  
- **Limit the number of plugins** → The more plugins you install, the greater the surface for potential issues. Stick to those that are essential to your workflow.  
- **Review permissions** → Some plugins request access to system features or external APIs. Make sure you understand what they do before enabling them.  

By keeping plugins **curated, updated, and documented**, you ensure both the **security and stability** of your company’s vault.
# Best Practices for Working with Obsidian
To make the most out of Obsidian, it’s important to adopt consistent habits that keep the vault organized, scalable, and easy to navigate. Here are some best practices:
- **Use clear and consistent naming for notes**  
  Choose descriptive titles and stick to a naming convention (e.g., `Project_Name – Meeting_Notes`) to make searching and linking easier.
- **Balance folders, tags, and links**  
  Use folders for broad categories, tags for transversal themes, and internal links to connect ideas. A mix of these methods creates flexibility without clutter.
- **Create hub notes for complex topics**  
  Summarize related atomic notes in a central hub note. This makes it easier to navigate large topics and provides context at a glance.
- **Leverage backlinks and the graph view**  
  Use backlinks to discover hidden relationships and the graph view to visualize clusters of knowledge. This helps you see the bigger picture beyond individual notes.

By following these practices, the vault becomes not just a storage system, but a **living network of knowledge** that supports both individual work and team collaboration.
