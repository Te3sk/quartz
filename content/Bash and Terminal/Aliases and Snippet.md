---
title: Aliases and Snippet
date: 2025-08-10
tags:
  - snippet
  - zsh
  - terminal
category: Bash and Terminal
status: in_corso
author: Te3sk
description: This document explain how create snippet in unix terminal, including alias and bash functions
---
**Aliases and shell snippets** are custom commands you define in your shell’s configuration file (`.bashrc` for Bash, `.zshrc` for [[Bash and Terminal#ZSH|Zsh]]) to save time and reduce repetitive typing.  
An **alias** replaces a long or complex command with a short keyword (e.g., `alias gs='git status'`), while a **snippet function** can combine multiple commands or add logic, acting like a small reusable script.  
The main benefit is speed and efficiency: frequently used commands become faster to type, less prone to mistakes, and easier to remember.  
Both Bash and Zsh support aliases and functions in the same way for most use cases, though Zsh offers more advanced autocompletion and can handle some pattern matching for aliases that Bash cannot.  
In either shell, these customizations load automatically when you open a terminal, becoming part of your everyday workflow.
## Setup your aliases
To set up an alias, first open your shell’s configuration file in a text editor.  
For **Bash**, edit `~/.bashrc`; for **Zsh**, edit `~/.zshrc`. You can use `nano` for a quick edit, for example:  
```bash
nano ~/.bashrc
```
or 
```bash
nano ~/.zshrc
```
Inside the file, add your alias using the syntax:
```bash
alias short_command='full command here'
```
Save and close the file, then reload your shell so the changes take effect:
```bash
source ~/.bashrc   # for Bash
source ~/.zshrc    # for Zsh
```
Finally, test your alias by typing its name in the terminal to confirm it runs the intended command.
If you want, I can also add **a short troubleshooting tip** here for when aliases don’t seem to work after setup. That would make the vault entry more complete.
## Set up snippet function
In `.bashrc` or `.zshrc` file, you can also write [[Bash Functions]] and load them automatically when the shell starts.  
This allows you to create more powerful and flexible snippets than simple aliases, as functions can take arguments, include conditional logic, and combine multiple commands.
You can also set up aliases about functions, for example:
```bash
my_function() {
	[......]
}

alias mf='my_function'
```
Once added, save the file and reload the shell with:
```bash
source ~/.bashrc   # for Bash
source ~/.zshrc    # for Zsh
```
From that point on, your custom functions will be available in every new terminal session, just like aliases.