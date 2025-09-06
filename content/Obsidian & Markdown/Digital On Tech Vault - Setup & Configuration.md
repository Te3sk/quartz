---
title: Digital On Tech Vault - Setup & Configuration
date: 2025-09-06
tags:
  - obsidian
  - doc
  - internal-tools
category: Obsidian & Markdown
status: in_corso
author: Te3sk
description: Guide to installing, configuring, and maintaining the Digital On Tech Vault in Obsidian, including mandatory settings, recommended setup, collaboration practices, and troubleshooting.
---
# 1. Prerequisites
To access and work with the **Digital On Tech Vault**, each user must have their own **GitHub profile**.  
Access to the company repository is managed centrally: users must **request access from the company GitHub account**, providing their GitHub nickname so that permissions can be granted.  
Once access has been granted, the repository must be **downloaded (cloned) onto your computer**.  
It is recommended to place the cloned repository inside your system’s **Documents** folder to keep the vault organized and easy to locate.
# 2. Vault Setup
## 2.1. Installing Obsidian
[Obsidian Doc - Download and install Obsidian](https://help.obsidian.md/install)

1. Visit the official website: [https://obsidian.md](https://obsidian.md).  
2. Download the installer for your operating system (Windows `.exe`, macOS `.dmg`, or Linux `.AppImage`).  
3. Run the installer and follow the on-screen instructions.  
4. Once installed, launch Obsidian from your applications menu.  


For the **Digital On Tech Vault**, you must select the second option: **Open folder as vault**, and choose the `content` folder located inside the repository that was previously cloned from GitHub.  
This ensures you are working directly within the correct vault structure.
## 2.2. Obsidian Configuration
### 2.2.1. Mandatory Settings
To ensure consistency across the **Digital On Tech Vault**, every user must configure the following mandatory settings in Obsidian:
- **Base appearance**  
  - Select either *Light* or *Dark* mode depending on preference, but keep font size set to a readable standard (e.g., 14–16px).  
  - This guarantees documents remain visually consistent when shared during meetings or screen sharing.  
- **Core plugins required:** Activate the following built-in plugins:  
    - **Backlinks** → enables bidirectional navigation between notes.  
    - **Graph View** → provides a visual map of connections inside the vault.  
    - **Templates** → ensures structured and reusable note formats.  
- These plugins form the foundation of the shared workflow and must always remain enabled.  
- **File & link options**  
  - Set **new note location** to the designated default folder inside the vault.  
  - Use **internal link format** as `[[Note Title]]` to keep links uniform and compatible across the team.  

By applying these settings, the vault remains **coherent, standardized, and easy to navigate**, regardless of who is contributing.
### 2.2.2. Recommended Setup
To improve productivity and maintain a consistent workflow across the **Digital On Tech Vault**, we recommend the use of specific community plugins. Below are the currently adopted plugins, with a short overview of their purpose and usage guidelines.
#### 2.2.2.1. Advanced Tables
**Overview**:  
Enhances the default Markdown table editing experience in Obsidian by providing auto-formatting, Excel-like navigation, spreadsheet formulas, and commands for adding, moving, or aligning columns and rows.

**Usage**:  
- Start a table by typing `|` followed by the first heading, then press **Tab** to move to the next column.  
- Use **Enter** to create a new row.  
- Navigation shortcuts:  
  - `Tab` → next cell  
  - `Shift + Tab` → previous cell  
  - `Enter` → next row  
  - `Ctrl + Shift + D` → open table controls sidebar  
- Commands are also available in the **Command Palette** (`Cmd/Ctrl + P`).  
- Tables can be exported to CSV if needed.
#### 2.2.2.2. Table of Contents
**Overview**:  
Automatically generates a table of contents based on the headings of the current note. Useful for long documents or structured guides.

**Usage**:  
- Run the command **“Create full table of contents”** from the command palette.  
- Alternatively, generate a TOC for the next heading level only.  
- Default output style is a bullet list, but it can be changed to numbered via settings.  
- Recommended hotkeys:  
  - `Cmd + Shift + T` → full table of contents  
  - `Cmd + T` → TOC for the next heading level  
#### 2.2.2.3. Text Snippets
**Overview**:  
Allows you to define custom snippets that expand into text, metadata blocks, code templates, or symbols. Speeds up note-taking and ensures consistent formatting across the vault.

**Usage**:  
- By default, snippets can be expanded using **Cmd/Ctrl + Tab**, this shortcut is not recommended because it conflicts with Obsidian’s native tab navigation.  
- Go to **Settings → Community Plugins → Text Snippets → Hotkeys** to change the keyboard shortcut.  
- We suggest setting it to **Shift + Tab** for smooth usage without conflicts.  
- Snippets can also be triggered from the **Command Palette** (`Cmd/Ctrl + P`).  
- Placeholders such as `$tb$` (tabstop), `$nl$` (newline), and `$end$` (cursor position) allow for flexible templates.  
- Example: typing a keyword like `metadata` and expanding it generates a ready-to-fill document template.

**Currently defined snippets in the Digital On Tech Vault**:  
Copy the following snippets and paste them in the settings page of the plugin.
```
metadata : ---$nl$title: Nome del tuo documento$nl$date: YYYY-MM-DD$nl$tags:$nl$   - tag1$nl$   - tag2$nl$category: Categoria Principale$nl$status: bozza/completo/in_corso$nl$author: Il tuo nome (opzionale)$nl$related:$nl$   - [[Link Interno 1]]$nl$   - [[Link Interno 2]]$nl$description: Breve descrizione del contenuto del documento.$nl$---

code : `$tb$`

bcode : **`$tb$`**

ccode : ```$tb$$nl$$nl$```

rarrow : $\rightarrow$

rrarrow : $\implies$

ad : ```ad-$tb$$nl$$nl$```$nl$$end$

ad-def : ```ad-def$nl$$tb$$nl$```$nl$$end$

h1 : #

h2 : ##

h3 : ###

h4 : ####

h5 : #####

txt : \text{$tb$}

math : $$tb$$

mmath : $$$tb$$$
```

**Output of snippets:**
- `metadata` → YAML frontmatter template with title, date, tags, category, status, author, related notes, and description.  
- `code` → Inline code placeholder.  
- `bcode` → Bold inline code.  
- `ccode` → Code block template.  
- `rarrow` → Right arrow symbol (`→`).  
- `rrarrow` → Implies arrow symbol (`⇒`).  
- `ad` → Admonition block starter.  
- `ad-def` → Admonition definition block starter.  
- `h1` → Heading level 1 (`#`).  
- `h2` → Heading level 2 (`##`).  
- `h3` → Heading level 3 (`###`).  
- `h4` → Heading level 4 (`####`).  
- `h5` → Heading level 5 (`#####`).  
- `txt` → LaTeX text command (`\text{...}`).  
- `math` → Inline math formula (`$...$`).  
- `mmath` → Display math block (`$$$...$$$`).  
#### 2.2.2.4. Hotkeys and Workspace Preferences
- **Hotkeys**: Assign hotkeys for frequently used plugin commands (e.g., Table of Contents generation, snippet expansion). This reduces reliance on the command palette.  
- **Workspace layout**: Keep sidebars tidy, with **File Explorer**, **Search**, and **Backlinks** pinned in the left sidebar; reserve the right sidebar for **Graph View** and **Outline**.  
- **Consistency**: All team members should maintain a similar workspace configuration to minimize confusion during collaboration.
# 3. Collaboration Guidelines
To ensure the **Digital On Tech Vault** remains consistent, navigable, and collaborative, all contributors must follow the same organizational rules and editing practices.  

- **Folder and Indexing Structure**  
  The vault is organized into **folders for each macro-topic** (e.g., *Web Development*, *Web Analysis*).  
  Inside each folder, there must be an **index file** with the same name as the folder. This file introduces the macro-topic and provides an index of all related content within that section.  
- **Naming Conventions**  
  Notes and folders must use **clear, descriptive titles**. File names should reflect the actual content and avoid ambiguous terms.  
  Example: use `Google Analytics – Events` instead of `events-final`.  
- **Images and Assets**  
  All images must be stored in the **`Assets` folder**, located inside the `Content` directory.  
  Use a clear naming convention for images: combine the related topic with a short description, separated by a dash.  
  Example: `GoogleAnalytics-dashboard.png`, `Obsidian-graph-view.png`.  
- **Headers and Numbering**  
  All documents must follow a **hierarchical numbering system** for headers:  
  - Level 1 headers → `1`, `2`, `3`  
  - Level 2 headers → `1.1`, `1.2`, `2.1`, `2.2`  
  - Level 3 headers → `1.1.1`, `1.1.2`, etc.  
  This ensures structure is clear and consistent across all notes.  
- **Internal Links and Hub Notes**  
  Use `[[Internal Links]]` to connect related notes.  
  For complex topics, create **hub notes** that summarize and link to smaller, atomic notes, ensuring easy navigation across the vault.  
- **Tags for Transversal Classification**  
  Use **tags** (e.g., `#todo`, `#meeting`, `#reference`) to classify notes across different sections. Tags should be consistent and shared across the team to allow transversal filtering.  
- **Best Practices for Clean Documentation**  
  - Avoid duplicate notes by linking to existing ones.  
  - Keep notes atomic and focused on a single topic.  
  - Regularly review and refactor hub notes to reflect updates.  
  - Do not overload folders with unrelated files—place content where it logically belongs.  

By applying these guidelines, the vault remains a **living, coherent knowledge base** that can scale with the team and be easily understood by both current and future contributors.

    

---

## 5. Maintenance & Updates

- Keeping Obsidian and plugins up to date.
    
- Periodic review of vault structure and consistency.
    
- Referencing Git documentation for repository sync and conflict management.
    

---

## 6. Troubleshooting (Obsidian Only)

- Common issues with Obsidian (e.g., missing plugins, display problems, corrupted settings).
    
- How to reset the workspace layout.
    
- How to disable or remove problematic community plugins.
    
- Reference to Git troubleshooting (handled in a separate document).