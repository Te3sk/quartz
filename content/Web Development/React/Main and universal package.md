---
title: Main package for all version of React
date: 2025-08-05
tags:
  - react
  - front-end
category: React Development
status: in_corso
author: Te3sk
description: This doc contain the main and most important package. Those package are independent from the version and the framework we choose to use
---
* [[#React e React-Dom]]
	* [[#React]]
	* [[#React-Dom]]
# React and React-DOM
When you create a React application, the first two essential packages that are always installed are **`react`** and **`react-dom`**. Even though we often use them without thinking much about it, it's important to understand what they do and why both are necessary.
## React
	Responsible for component logic and structure
The `react` package includes everything you need to **create components**, **manage state**, **use hooks**, and generally **build the UI logic** of your application using JSX. In other words, it's the **core engine of React**.
When you use things like:
```jsx
import React, { useState, useEffect } from 'react';
```
or:
```jsx
return <MyComponent />;
```
...you're using features provided by this package.
## React-DOM
	Responsible for rendering to the browser
The `react-dom` package is what **connects React to the browser**. It takes the component structure you've built with React and actually **renders it into the HTML page**. It translates your React component tree into real DOM elements.
The most common usage is:
```jsx
ReactDOM.render(<App />, document.getElementById('root'));
```
Or, with the modern API:
```jsx
import { createRoot } from 'react-dom/client';  const root = createRoot(document.getElementById('root')); root.render(<App />);
```
