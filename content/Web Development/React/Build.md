---
title: Build a React project for deploy
date: 2025-08-08
tags:
  - front-end
  - react
  - network
category: React Development
status: completed
author: Te3sk
description: Guide to build a react and create a directory ready to deploy
---
- [[#Premises|Premises]]
- [[#Build the project|Build the project]]
- [[#Test the project|Test the project]]
- [[#Compress the project|Compress the project]]

This section focuses on the **build process for React applications**, typically triggered by running `npm run build`. This command uses tools like Vite, Webpack or similar bundlers (depending on your setup) to compile and optimize the source code — including React components, JavaScript modules, CSS, and assets — into a static, production-ready bundle. The output is a set of highly optimized files (HTML, CSS, JavaScript) that can be served by any static web server, ensuring **fast loading times**, **better caching**, and **improved performance** for end-users.
## Premises
We should have a directory like the following:
```bash
$ tree [project]

.
├── index.html
├── package-lock.json
├── package.json
├── public
│   └── assets
|		└── ...
├── src
│   ├── App.tsx
│   ├── NotFound.tsx
│   ├── main.tsx
│   ├── index.css
│   ├── components
│	│	└── ...
│   ├── context
│	│	└── ...
│   ├── hooks
│	│	└── ...
│   ├── i18n
│	│	└── ...
│   ├── lib
│	│	└── ...
│   ├── models
│	│	└── ...
│   ├── pages
│	│	└── ...
│   ├── styles
│	│	└── ...
│   ├── types
│	│	└── ...
│   └── vite-env.d.ts
├── tailwind.config.js
├── tsconfig.app.json
├── tsconfig.json
└── vite.config.ts
```
## Build the project
In the project directory, run the command:
```bash
npm run build
```
This will create the `dist` (or `build`, depending on your setup) subdirectory containing the **production version** of the project.
In React, the build output is a **fully static set of files** (HTML, CSS, JS) that can be deployed directly to any static hosting service (e.g. Vercel, Netlify, Firebase Hosting, or traditional web servers).
Unlike server-side frameworks like Express, there's **no need to include runtime dependencies or `.env` files** in production. The environment variables must be baked into the build at compile time (e.g., via `REACT_APP_` prefixed variables), and the output is self-contained and ready to serve.
## Test the project
You can also check if the production folder work running:
```bash
npm run preview
```
This command serves the **compiled production build** locally, allowing you to test it in an environment that closely mimics real deployment. It’s especially useful for catching layout or routing issues that might not appear during development with `npm run dev`.
Note that `npm run preview` is available in tools like **Vite**, but may differ or require setup if you're using **Create React App** or other build systems.
## Compress the project
If the production version works correctly, we can compress it into an archive to facilitate portability and be able to easily move it to where we will deploy:
```bash
tar -czvf built-project.tar.gz dist
```
This archive contains everything needed to serve the app in production.  
You can now safely transfer it to your target server or hosting service and extract it there for deployment.