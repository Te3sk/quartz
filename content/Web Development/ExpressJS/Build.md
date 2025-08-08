---
title: React Building version
date: 2025-07-29
tags:
  - back-end
  - expressJS
category: ExpressJS
status: completed
author: Te3sk
description: How to make the build version and how to handle it
---
This section focuses on the **build process**, typically initiated by `npx tsc`. This command compiles and optimizes your application's source code (e.g., React components, JavaScript, CSS) into a highly efficient, production-ready bundle. The output is a set of static files (HTML, CSS, JavaScript) that are minimized, compressed, and ready for deployment to a web server, ensuring faster loading times and better performance for end-users.

## Premises
We should have a directory like the following:
```bash
$ tree project     

project
├── node_modules
│   └── ...
├── logs
│   └── ...
├── package-lock.json
├── package.json
├── src
│   ├── app.ts
│   ├── config
│   │   └── ...
│   ├── controllers
│   │   └── ...
│   ├── logger
│   │   └── ...
│   ├── middlewares
│   │   └── ...
│   ├── models
│   │   └── ...
│   ├── routes
│   │   └── ...
│   ├── server.ts
│   ├── services
│   │   └── ...
│   └── utils
│       └── ...
└── tsconfig.json
```
# Build the project
Running the command
```bash
npx tsc
```
will create the `dist` subdirectory which contain the **production version** of the project. 

To make the production folder completely independent from the rest of the directory, we have to install the production dependency in the `dist folder`:
```bash
cd project
npm install --production
```
# Test the project
Once we have `dist`, we can run the command
```bash
node server.js
```
to run a local static web server that serves the production-ready version.
# Compress the project
If the production version works correctly, we can compress it into an archive to facilitate portability and be able to easily move it to where we will deploy:
```bash
cd ..
tar -czvf built-project.tar.gz dist package.json package-lock.json node_modules
```
Including the `node_modules` directory and `package.json` files in the deployment archive ensures that all necessary dependencies and exact package versions are bundled together with the application code. This guarantees that the server environment runs the application with the same libraries and configurations as the development environment, avoiding issues caused by missing or mismatched dependencies. While it increases the archive size, this approach simplifies deployment and reduces the risk of runtime errors due to inconsistent or incomplete installations.
**Important:** if the project include an `.env` file, it must be included in the compressed archive.

Now we have the archive ready to move in our project directory.