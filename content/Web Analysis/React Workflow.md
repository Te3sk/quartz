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
## PageView - push in dataLayer route changes
Create a GMT helper that send events data to GMT `dataLayer`:
```ts title="../src/lib/gmt.ts"
declare global { interface Window { dataLayer: {[key: string]: any}[]} }

export function pushGtm(eventName: string, params: Record<string, any> = {}) {
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({ event: eventName, ...params });
}
```
And create a component that intercept the changes of course in React Router and sends a `virtual_pageview` to any navigation, with URL, path and page title:
```tsx title="../src/Hooks/RouteChangeTrack.tsx
import { useEffect, useRef } from "react";
import { useLocation } from "react-router-dom";
import { pushGtm } from "./lib/gtm";

export default function RouteChangeTracker() {
  const location = useLocation();
  const prevUrlRef = useRef<string>();

  useEffect(() => {
    const url = window.location.href;
    if (prevUrlRef.current === url) return; // evita duplicati
    prevUrlRef.current = url;

    pushGtm("virtual_pageview", {
      page_location: url,
      page_path: location.pathname + location.search + location.hash,
      page_title: document.title || undefined,
    });
  }, [location.pathname, location.search, location.hash]);

  return null;
}
```
Finally integrate the component in `App.jsx`:
```tsx {4}
import RouteChangeTracker from "./RouteChangeTracker";
/* ... */
<Router>
  <RouteChangeTracker />
  {/* .... */}
</Router>
```
After write and deploy those component, [[Google Analytics#Config Google Tag Manager|config Google Tag Manager]].
## Send event from the app
After defining the [[Google Analytics#Define Event taxonomy|Event Taxonomy]], you have to implement the tracker in the app. Create a library file in the `/lib` folder or create a `/analytics` or `/lib/analytics` (depend on your project) named `events.ts`. This file should have a function for each event defined in the [[Web Analysis#**Analytics Documentation – Structure Overview**|documentation]] and each function should be like the following:
```typescript title="events.ts"
import { pushGtm } from "./gmt";
const sent = new Set();

function track_event(event, params = {}) {
  // 1) enrich
  const enriched = {
    page_location: location.href,
    currency: "EUR",
    ...params,
  };

  // 2) sanitize / validate
  if (enriched.email) delete enriched.email; // no PII
  // validate types minimally

  // 3) dedup (optional)
  const key = `${event}:${enriched.transaction_id ?? ""}`;
  if (event === "purchase" && sent.has(key)) return;
  if (event === "purchase") sent.add(key);

  // 4) send
  pushGMT("[event_name]", enriched)
}
```
