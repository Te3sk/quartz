---
title: Google Tag Manager
date: 2025-08-11
tags:
  - analytics
  - network
  - api
category: Web Analysis
status: completed
author: Te3sk
description: A tag management system that lets you deploy and update tracking codes (tags) without editing your website’s codebase directly.
---
- [[#Introduction|Introduction]]
- [[#Installation|Installation]]
	- [[#1. **Create a Container**|1. Create a Container]]
	- [[#2. **Get configuration snippet**|2. Get configuration snippet]]
	- [[#3. **Deploy and test**|3. Deploy and test]]
## Introduction
**Google Tag Manager (GTM)** is a free tag management system by Google that allows you to add, update, and manage tracking codes (tags) on your website or app without directly modifying the source code.  
It’s important because it centralizes all your [[Google Analytics|analytics]], [[Google Ads|advertising]], and custom tracking scripts in one interface, reducing deployment time and minimizing the risk of errors from manual code edits.  
GTM works by embedding a small container script in your site’s HTML; inside the GTM dashboard, you define tags (e.g., Google Analytics, Meta Pixel), triggers that decide when those tags fire (e.g., page load, button click), and variables that store dynamic values.  
This approach enables marketers and developers to deploy and adjust tracking implementations quickly, without requiring a full development cycle for every change.
## How GMT Works
There are **3 main instances:**
* **Tags:** a measurement code
* **Triggers:** the conditions when we want to fire measurement codes
* **Variables:** any data we can collect from the website or push in the Data Layer and collect to the tags
## Setup
### Install the GMT Manager Code
First you have to create the **connection between GMT and your website**. To do that, in [GMT Workspace](https://tagmanager.google.com) **create a container** and then click `Admin > Install Google Tag Manager`. There  you can find 2 HTML tag and you have to paste them in the HTML files of your website (each page), one in the `<head>` and the other in the `<body>`.
### GA4 Configuration Tag
Now click on `New Tag`
## --------- OLD ---------
## Installation
First login with the right google account and go to [GMT Workspace](https://tagmanager.google.com).
#### 1. **Create a Container**
Click on **`Create Account`** and fill in all the fields with the required information.
#### 2. **Get configuration snippet**
In your **GMT Workspace** click on **`Admin`** $\implies$ **`Install Google Tag Manager`**. Now you will see 2 snippet like those:
```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-AAA11AAA');</script>
<!-- End Google Tag Manager -->

<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-AAA11AAA"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
```
Those script needs to be copied and pasted in the root html file of your site (usually `index.html`). You have to paste the first script in the `<head>` and the second in the `<body>`, both as high as possible.
#### 3. **Deploy and test**
After integrating the scripts save, launch or deploy the app. In GMT workspace click on **`Preview`** (top right) to check that the container is loading. If it doesn't connect, try to disable ad-blocker, check the container-id in the snippet and check the URL is reachable.
When you're ready, click **`Submit`** $\implies$ **`Publish`** in GTM to put your changes live.