---
title: Bash and Terminal
date: 2025-08-10
tags:
  - bash
  - terminal
  - unix
  - snippet
  - zsh
category: Bash and Terminal
status: in_corso
author: Te3sk
description: work efficiently with the terminal, create snippets and shortcut functions, write bash scripts to speed up work
---
# Summary
* [[Aliases and Snippet]]
* [[Bash Functions]]
# Introduction
This section focuses on helping you **work efficiently with the terminal**, whether you’re running quick commands, automating tasks, or building complex workflows.  
It brings together essential knowledge, practical tips, and ready-to-use examples so you can navigate faster, write less repetitive code, and spend more time on meaningful development work.
# UNIX Terminal
The **UNIX terminal** (or shell) is a text-based interface that lets you interact directly with the operating system.  
Instead of using graphical menus, you type commands to perform actions such as navigating the file system, manipulating files, running programs, managing processes, or connecting to remote servers.  
The terminal works by interpreting your input through a shell program (like Bash, Zsh, or Fish), which parses commands, executes them, and displays the output.  
It is widely used by developers and system administrators because it’s faster, scriptable, and often more powerful than graphical tools.
## ZSH
**Zsh** (Z Shell) is an extended UNIX shell that builds upon the features of Bash while adding powerful enhancements for interactivity, customization, and scripting.  
Like other shells, Zsh acts as a command interpreter: it takes the commands you type, parses them, and executes them via the operating system.  
It supports advanced tab completion, spelling correction, better globbing (pattern matching for filenames), and a rich set of options for customizing the prompt and behavior.  
Zsh also integrates well with frameworks like **Oh My Zsh**, which provide pre-built themes, plugins, and aliases to streamline the terminal experience.  
For many developers, switching to Zsh means gaining a more productive, visually clear, and highly configurable working environment.
## Bash vs Zsh
The main difference lies in the shell program your terminal runs: **Bash** is the default on many Linux servers, while **Zsh** is the default on macOS. Bash is widely supported, stable, and available almost everywhere, making it ideal for scripts and cross-platform compatibility. Zsh offers all Bash features plus advanced autocompletion, better globbing, and extensive customization, especially when paired with frameworks like *Oh My Zsh*. The trade-off: Bash is simpler and more universal but less feature-rich, while Zsh is more powerful and user-friendly but not always installed by default on remote systems.
# Bash functions
**Bash functions** are reusable blocks of shell commands that you can define once and call anytime from your terminal.  
They work much like functions in any programming language: you give them a name, optionally pass arguments, and they execute a predefined set of instructions.  
Bash functions are useful for automating repetitive tasks, creating command shortcuts, or combining multiple commands into a single, more powerful operation.  
Once defined in your shell configuration file (e.g., `.bashrc` or `.bash_profile`), they can be available every time you open a terminal, effectively extending the capabilities of the shell to fit your personal workflow.