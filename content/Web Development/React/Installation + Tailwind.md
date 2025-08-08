---
title: Installation of Vite React + TailwindCSS
date: 2025-08-05
tags:
  - front-end
  - react
  - tailwind
category: React Development
status: in_corso
author: Te3sk
description: Installation and setup of a Vite React with TailwindCSS framework
Vite React Link: https://vite.dev/guide/#scaffolding-your-first-vite-project
TailwindCSS: https://tailwindcss.com/docs/installation/using-vite
---
# Installation
## Create your project
Start by creating a new Vite project if you don’t have one set up already. The most common approach is to use [Create Vite](https://vite.dev/guide/#scaffolding-your-first-vite-project).
```bash
npm create vite@latest my-project
cd my-project
```
## Install Tailwind CSS
Install `tailwindcss` and `@tailwindcss/vite` via npm.
```bash
npm install tailwindcss @tailwindcss/vite
```
# Configuration
## Configure the Vite plugin
Add the `@tailwindcss/vite` plugin to your Vite configuration in `vite.config.ts` file. If that file doesn't exists, create it in the project route.
```js
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({
	plugins: [
		tailwindcss(),
	],
})
```
## Import Tailwind CSS
Add an `@import` to your CSS file that imports Tailwind CSS. Typically the file is `App.css` or `index.css`.
```css
@import "tailwindcss";
```
