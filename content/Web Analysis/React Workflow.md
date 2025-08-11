---
title: React Workflow for analytics
date: 2025-08-11
tags:
  - analytics
  - network
  - react
category: Web Analysis
status: in_corso
author: Te3sk
description: Full workflow to integrate all the analytics tool in a react app
---

## Google Tag Manager - GMT
Follow all the instruction to [[Google Tag Manager#Installation|setup GMT]]. Take the GMT snippets and paste them into the `index.html` file located in the project route (outside the `src/` folder). You should have something like this:
```html
<!DOCTYPE html>

<html lang="en">
	<head>
		[...]
		<!-- Google tag (gtag.js) -->
		<!-- Loads Google Analytics 4 (GA4) directly and sends pageview and event data using the gtag function. -->
		<script async src="https://www.googletagmanager.com/gtag/js?id=G-PTTJVEZ5JM"></script>
		<script>
		window.dataLayer = window.dataLayer || [];
		function gtag() { dataLayer.push(arguments); }
		gtag('js', new Date());
		gtag('config', 'G-AAA11AAA');
		</script>
		<!-- Google Tag Manager -->
		<!-- Loads the Google Tag Manager script and initializes the dataLayer to manage all tracking tags centrally. -->
		<script>(function (w, d, s, l, i) {
		w[l] = w[l] || []; w[l].push({
		'gtm.start':
		new Date().getTime(), event: 'gtm.js'
		}); var f = d.getElementsByTagName(s)[0],
		j = d.createElement(s), dl = l != 'dataLayer' ? '&l=' + l : ''; j.async = true; j.src =
		'https://www.googletagmanager.com/gtm.js?id=' + i + dl; f.parentNode.insertBefore(j, f);
		})(window, document, 'script', 'dataLayer', 'GTM-AAA11AAA');</script>
	</head>
	<body>
		<div id="root"></div>
		<script type="module" src="/src/main.tsx"></script>
	</body>
</html>
```
