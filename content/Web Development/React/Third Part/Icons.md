---
title: Icons Packs for React
date: 2025-08-05
tags:
  - react
  - front-end
  - ui
  - icons
category: React Development
status: in_corso
author: Te3sk
description: Installation and usage of the main icons packs for React
---
* [[#React Icons]]
* [[#Lucide React]]
* [[#Material Icons]]
# React Icons
## Installation
First install the package in the project directory:
```bash
npm install react-icons
```
Once the package is installed you can find a lot of icons of different styles in [react-icons website](https://react-icons.github.io/react-icons/)
## Usage
The basic usage is:
```jsx
import { FaBeer } from "@react-icons/all-files/fa/FaBeer";

function Question() {
	  return (
	    <h3>
	      Lets go for a <FaBeer />?
	    </h3>
	);
}
```

## Context usage
If, on the other hand, you have **multiple icons** that share the **same style** (same color, size, className, etc.), instead of repeating props on each icon, you should use `IconContext.Provider`. This way, you define the style **once**, and all child icons automatically inherit it.
Context provides a way to pass data through the component tree without having to pass props down manually at every level.
```jsx
import { IconContext } from "react-icons";

<IconContext.Provider value={{ color: "blue", className: "global-class-name" }}>
	<div>
		<FaFolder />
	</div>
</IconContext.Provider>;
```

| Keys        | Default               |                                    |
| ----------- | --------------------- | ---------------------------------- |
| `color`     | `undefined` (inherit) |                                    |
| `size`      | `1em`                 |                                    |
| `className` | `undefined`           |                                    |
| `style`     | `undefined`           | Can overwrite size and color       |
| `attr`      | `undefined`           | Overwritten by other attributes    |
| `title`     | `undefined`           | Icon description for accessibility |
# Lucide React
[Lucide React Documentation](https://lucide.dev/guide/)
## Installation
```bash
npm install lucide-react
```
Once the package is installed you can find a lot of icons in [Lucid Icons website](https://lucide.dev/icons/)
## Basic usage
```jsx
import { Smile } from "lucide-react";

function App() {
  return (
    <div className="app">
      <Smile />
    </div>
  );
}
```

* There are 2 ways to edit **color property**: using the relative `prop` or using parent element text color value. In the second case you can use both css and tailwindCSS
```jsx
<div className="app">
	{/* color prop */}
	<Smile color="#3e9392" />

	{/* parent element text-color value */}
	<button style={{ color: "#fff" }}>
	    <Smile />
	    Like
    </button>
</div>
```
* There are 4 ways to edit **size property**: using the relative `prop`, via CSS, via CSS based on the font size or with tailwind
```jsx
<div className="app">
    {/* size prop */}
	<Landmark size={64} />

	{/* with tailwind */}
	<PartyPopper className="w-24 h-24" />
</div>
```
```css
/* Via CSS */
.my-beer-icon {
  width: 64px;
  height: 64px;
}

/* Via CSS dynamically change the icon size based on the font size */
.my-icon {
  /* Icon size will relative to font-size of .text-wrapper */
  width: 1em;
  height: 1em;
}

.text-wrapper {
  /* Change this! */
  font-size: 96px;

  /* layout stuff */
  display: flex;
  gap: 0.25em;
  align-items: center;
}
```
* When adjusting the `size` prop the size of the stroke width will be relative to the size of the icon, this is the default SVG behavior. The `absoluteStrokeWidth` prop is introduced to adjust this behavior to make the stroke width constant no matter the size of the icon.
```jsx
<div className="app">
	<FolderLock strokeWidth={1} />

	<FolderLock 
		strokeWidth={96} 
		absoluteStrokeWith={true}
	/>
</div>
```
## Global Styling
Styling icons is easy to accomplish using CSS.
Every icon has a class attribute applied called `lucide`. This class name can be used in the CSS file to target all icons that are being used within the app.
```css
.lucide {
  color: #ffadff;
  width: 56px;
  height: 56px;
  stroke-width: 1px;
}
```
For global absolute stroke width styling the `vector-effect: non-scaling-stroke` CSS property can be applied to the children. This will keep the stroke-width the same size no matter the size of the icon. 
```css
.lucide * {
  vector-effect: non-scaling-stroke;
}
```
## Filled Icons

Fills are officially not supported. However, all SVG properties are available on all icons. Fill can still be used and will work fine on certain icons.
Example with stars:
```jsx
<div className="app">
	<div className="star-rating">
		<div className="stars">
			{ Array.from({ length: 5 }, () => (
				<Star fill="#111" strokeWidth={0} />
			))}
		</div>
		<div className="stars rating">
			<Star fill="yellow" strokeWidth={0} />
			<Star fill="yellow" strokeWidth={0} />
			<StarHalf fill="yellow" strokeWidth={0} />
		</div>
	</div>
</div>
```
# Material Icons
[Material Icons Documentation](https://mui.com/material-ui/icons/)
Google has created over 2,100 official [Material icons](https://fonts.google.com/icons?icon.set=Material+Icons), each in five different "themes" (see below). For each SVG icon, we export the respective React component from the `@mui/icons-material` package.
## Installation
```bash
npm install @mui/icons-material
```
## Basic Usage
Import the icons in the component:
```jsx
import { AccessAlarm, ThreeDRotation } from '@mui/icons-material';

function App() {
  return (
    <div className="app">
      <AccessAlarm />
      <ThreeDRotation />
    </div>
  );
}
```
## Properties
The available properties are:
- `color`: Sets the icon color. You can use predefined values like `"inherit"`, `"primary"`, `"secondary"`, `"action"`, `"error"`, `"disabled"`, or the hex code (`"[#ffffff]"`)
- `fontSize`: Adjusts the icon size. Accepts values like `"inherit"`, `"small"`, `"medium"`, or `"large"`.
- `fill`: Defines the fill color of the icon. Set to `"none"` to disable filling.
- `viewBox`: Specifies the position and dimension of the SVG viewport. Useful for scaling the icon correctly.
- `strokeWidth`: Sets the width of the stroke lines in the icon.
- `stroke`: Defines the stroke color. `"currentColor"` makes it inherit from the text color. 
```jsx
<HomeIcon 
	color="primary"
	fontSize="small"
	fill="none"
    viewBox="0 0 24 24"
    strokeWidth={1.5}
    stroke="currentColor"
/>
```
## SvgIcons
If you need a custom SVG icon (not available in the [Material Icons](https://mui.com/material-ui/material-icons/)) you can use the `SvgIcon` wrapper. This component extends the native `<svg>` element:
- It comes with built-in accessibility.
- SVG elements should be scaled for a 24x24px viewport so that the resulting icon can be used as is, or included as a child for other Material UI components that use icons. This can be customized with the `viewBox` attribute. To inherit the `viewBox` value from the original image, the `inheritViewBox` prop can be used.
- By default, the component inherits the current color. Optionally, you can apply one of the theme colors using the `color` prop.
- It supports `<svg>` element as a child so you can copy and paste your SVG directly to `SvgIcon` component.
```jsx
<SvgIcon>
	{/* credit: cog icon from https://heroicons.com */}
	<svg
		xmlns="http://www.w3.org/2000/svg"
		fill="none"
		viewBox="0 0 24 24"
		strokeWidth={1.5}
		stroke="currentColor"
	>
	    <path
		    strokeLinecap="round"
		    strokeLinejoin="round"
		    d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495"
		/>
	</svg>
</SvgIcon>
```
