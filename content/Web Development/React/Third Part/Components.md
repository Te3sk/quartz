---
title: Ready-to-use React Components
date: 2025-08-05
tags:
  - front-end
  - react
  - ui
category: React Development
status: in_corso
author: Te3sk
description: Installation and usage of some library that provide ready-to-use react components
---
- [[#ShadCn|ShadCn]]
	- [[#ShadCn#Installation Vite React|Installation Vite React]]
	- [[#ShadCn#Component Usage|Component Usage]]
	- [[#ShadCn#Blocks and Charts Usage|Blocks and Charts Usage]]
- [[#Material UI - MUI|Material UI - MUI]]
	- [[#Material UI - MUI#Installation|Installation]]
	- [[#Material UI - MUI#Usage|Usage]]
- [[#FlowBite|FlowBite]]
	- [[#FlowBite#Integration in Vite React|Integration in Vite React]]
	- [[#FlowBite#Usage|Usage]]
## ShadCn
[ShadCn Documentation](https://ui.shadcn.com/docs)
	[ShadCn Components](https://ui.shadcn.com/docs/components)
	[ShadCn Blocks](https://ui.shadcn.com/blocks)
	[ShadCn Charts](https://ui.shadcn.com/charts)
shadcn/ui is a set of beautifully-designed, accessible components and a code distribution platform. Works with your favorite frameworks and AI models. Open Source. Open Code.
### Installation Vite React
This library needs the previous [[Installation + Tailwind|installation of the Vite React project with TailwindCSS]].
1. **Edit tsconfig.json file:** The current version of Vite splits TypeScript configuration into three files, two of which need to be edited. Add the `baseUrl` and `paths` properties to the `compilerOptions` section of the `tsconfig.json` and `tsconfig.app.json` files:
```json
// tsconfig.json
{
	"files": [],
	"references": [
		{
			"path": "./tsconfig.app.json"
		},
		{
			"path": "./tsconfig.node.json"
		}
	],
	"compilerOptions": {
		"baseUrl": ".",
		"paths": {
		"@/*": ["./src/*"]
		}
	}
}
```
2. **Edit tsconfig.app.json file:** Add the following code to the `tsconfig.app.json` file to resolve paths, for your IDE:
```json
// tsconfig.app.json
{
	"compilerOptions": {
	// ...
		"baseUrl": ".",
		"paths": {
			"@/*": [
				"./src/*"
			]
		}
		// ...
	}
}
```
3. **Update vite.config.ts:** Add the following code to the vite.config.ts so your app can resolve paths without error:
```bash
npm install -D @types/node
```
```ts
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

// https://vite.dev/config/
export default defineConfig({
	plugins: [react(), tailwindcss()],
	resolve: {
		alias: {
			"@": path.resolve(__dirname, "./src"),
		},
	},
})
```
4. **Run the CLI:** Run the `shadcn` init command to setup your project:
```bash
npx shadcn@latest init
```
You will be asked a few questions to configure `components.json`:
```bash
Which color would you like to use as base color? › Neutral
```
### Component Usage
First a component from [ShadCn components list](https://ui.shadcn.com/docs/components), then add the component to your project:
```bash
npx shadcn@latest add button
```
Now you can use it in your code:
```jsx
import { Button } from "@/components/ui/button"

export default function Home() {
	return (
		<div>
			<Button>Click me</Button>
		</div>
	)
}
```
	change Button with the component you choosen
	
All components have variants shown and listed on the component specific page
### Blocks and Charts Usage
[ShadCn Blocks list](https://ui.shadcn.com/blocks)
[ShadCn Charts list](https://ui.shadcn.com/charts)

Both Blocks and charts has different implementation depends on the single component (block or chart) you want. Just check the documentation page of the component you choose.
## Material UI - MUI
Material UI is an open-source React component library that implements Google's Material Design. It's comprehensive and can be used in production out of the box.

Material UI is an open-source React component library that implements Google's [Material Design](https://m2.material.io/).
It includes a comprehensive collection of prebuilt components that are ready for use in production right out of the box and features a suite of customization options that make it easy to implement your own custom design system on top of our components.

It provide components and [[Icons#Material Icons|Icons]]
### Installation
This library needs the previous installation of [[Main and universal package#React e React-Dom|react e react-dom]].
```bash
npm install @mui/material @emotion/react @emotion/styled
```
Material UI uses the [Roboto](https://fonts.google.com/specimen/Roboto) font by default. Add it to your project via Fontsource, or with the Google Fonts CDN.
```bash
npm install @fontsource/roboto
```
Then you can import it in your css entry point, typically `index.css` like this:
```css
import '@fontsource/roboto/300.css';
import '@fontsource/roboto/400.css';
import '@fontsource/roboto/500.css';
import '@fontsource/roboto/700.css';
```
### Usage
You can choose a component from the [MUI component list](https://mui.com/material-ui/all-components/) and follow the instruction to use and customize it. Each component has a lot of different customizable properties.
In the documentation page there are instruction to:
* [Customize components](https://mui.com/material-ui/customization/how-to-customize/) (dark-mode, theme, palette, typography, ...)
* [Minimize bundle size](https://mui.com/material-ui/guides/minimizing-bundle-size/)
* [Make responsive ui](https://mui.com/material-ui/guides/responsive-ui/)
* [Testing](https://mui.com/material-ui/guides/testing/)
* ... and much more
## FlowBite
[Flowbite documentation page](https://flowbite-react.com/docs/getting-started/introduction)
[Flowbite React](https://github.com/themesberg/flowbite-react) is a comprehensive UI component library that brings together the power of React and the utility-first approach of Tailwind CSS. Built on top of the core Flowbite components, it provides a robust foundation for creating modern, responsive web applications.
### Integration in Vite React
This library need the previous installation of [[Installation + Tailwind|Vite React project with TailwindCSS framework]].
```bash
npx flowbite-react@latest init
```
This will automatically:
- Install Flowbite React and its dependencies
- Configure Tailwind CSS to include Flowbite React plugin
- Set up necessary configurations
### Usage
After choose a component to use in your app, just import it in the parent component
```jsx
import { Accordion } from "flowbite-react";
```
and use it as explained in the documentation. 
Each component have different features and implementations, check the documentations.