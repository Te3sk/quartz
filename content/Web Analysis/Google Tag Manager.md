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
[Google press official releases regarding GA4](https://support.google.com/analytics/answer/9164320#071122&zippy=%2Creleases)
### Create and Configure Account
**TODO**
### Install the GMT Manager Code
First you have to create the **connection between GMT and your website**. To do that, in [GMT Workspace](https://tagmanager.google.com) **create a container** and then click `Admin > Install Google Tag Manager`. There  you can find 2 HTML tag and you have to paste them in the HTML files of your website (each page), one in the `<head>` and the other in the `<body>`.
### GA4 Configuration Tag
Now click on `New Tag`, name it *GA4 - Configuration Tag* and in `Tag Configuration` choose `Google Analytics: GA4 Configuration`. Now you have to paste the `Measurement ID` from [[Google Analytics#GA4 Data Stream|Data Stream Details]] in your [Google Analytics Workspace](https://analytics.google.com/analytics/). 
Now you have to select a **Trigger**, or when you want to fire the measurement. There are couple of predefined triggers, if you want to measure **all the website** click on `All Pages`. 
Click on `Save` and then to `Submit` to make the changes work.

Depending on the version of GMT, `Google Analytics: GA4 Configuration` may be **deprecated** and you wont find it. In this case, choose `Google Tag` as **Tag Type**. 
Now, in [Google Analytics Workspace](https://analytics.google.com/analytics/), go to `Admin > Data Stream > [your data stream] > Configure Tag Settings`; here, under *Your Google Tag*, click the Google tag (left side of diagram) and copy the **Google Tag ID** under *Tag Details*.
Return to the GMT tab and paste the **Google Tag ID** in the file that requires it.
As **trigger** choose `Initialization - All Pages`, in this way the tag will be loaded before the others tags (pageview or DOM).

### Events Configuration
There are way to **send event information** to GA4 by GMT:
* **Recommended Events:** those events are know by GMT, so you can choose the one you need from a list and easily set them up
* **Custom Events:** are events that GMT doesn't know and that are specific to your business. In this case you need a little more steps to set them up.
* **Push in DataLayer:** you have to use this way for the most critical events of your system. With *critical events* we mean events that are particularly important for analyzing your business and that need more specific configuration. Those events are pushed from the code of your website.
#### Recommended Events
First find the **component that trigger the event** in your project, let's use **clicking link** as example:
```html
<a class="class-name" id="component-id" href="[URL]">[...]</a>
```
1. **Variable:**
	Choose which property you want to use to identify the component (class, id, ...), then in [GMT Workspace](https://tagmanager.google.com) go to `Variables > Configure` and select the property you chosen in the *built-in variables* list that opens.
2.  **Trigger:** 
	Go to `Trigger > New` to create the trigger for this event. Choose a recognizable **name** and the right **Tag Type** from the list, since we are talking about recommended events, GMT will have a trigger type planned for the event you are setting up. In our example we will choose **`Just Links`**. Every Tag Type has their own configuration parameters, fill them according to your needs.
3. **Tag:**
	Now go to `Tag > New` to create the tag for this event.  After you name it, choose `Google Analytics: GA4 Event` as **Tag Type**, select the right **configuration tag** or insert you **Measurement ID** and choose the **event name** from the list (by clicking the icon left to the field), finally add **parameters** if needed. As **Trigger** select the one you created in step 2

Once you submit the new version, you will see the new events in GA4 reports.
#### Custom Events
It's an event that is specific to your business and GA doesn't know anything about it yet. There are a few extra steps you need to do to make the datas available in your report.
1. **Variable:** 
	Choose which property you want to use to identify the component (class, id, ...), then in [GMT Workspace](https://tagmanager.google.com) go to `Variables > Configure` and select the property you chosen in the *built-in variables* list that opens.
2. **Trigger:**
	Go to `Trigger > New` to create the trigger for this event. Choose a recognizable **name** and the right **Tag Type** from the list. Then use the additional settings (them depend on the tag type) to identify the component that trigger the event (id, className, ...).
3. **Tag:**
	Now go to `Tag > New` to create the tag for this event.  After you name it, choose `Google Analytics: GA4 Event` as **Tag Type**, select the right **configuration tag** or insert you **Measurement ID** and **name your event**  as you like (best practice is to use lowercase and to use `_` instead of spaces). Finally, set up **parameters** if you need, the best practice is to use them identify the component that trigger the event or identify the case/user-flow of the event.

Once you submit the new version, you will see the new events in GA4 reports.
#### DataLayer
[DataLayer Documentation](https://developers.google.com/tag-platform/tag-manager/datalayer)
The Data Layer is a JavaScript object (`window.dataLayer`) that acts as a central communication channel between your website or app and tracking tools like Google Tag Manager and GA4. It works by storing structured information about user actions and pushing events into a queue, which GTM can then read and forward to analytics or marketing platforms.
A simple example of a dataLayer object is:
```js title="DataLayer Example"
{
  event: "checkout_button",
  gtm: {
    uniqueEventId: 2,
    start: 1639524976560,
    scrollThreshold: 90,
    scrollUnits: "percent",
    scrollDirection: "vertical",
    triggers: "1_27"
  },
  value: "120"
}
```
##### Set up
To enable DataLayer pushing, you need to add some code before [[#Install the GMT Manager Code|GMT Manager Code]] like that:
```html {1-3, 12-17}
<script>
window.dataLayer = window.dataLayer || [];
</script>
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src= 'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-XXXXXX');</script>
<!-- End Google Tag Manager -->
```
##### Event Handlers
To push an event in the dataLayer, just use the built-in function:
```js
datalayer.push({"parameter-name" : "parameter-value"})
```
The good practice is to have `"event_name" : "[name]"` as first parameters.
### Scroll Tracking - Advanced Measurement Example 
First you have to create a **trigger**, to rule when you want to send the datas to GA4, by clicking on `Triggers > New`. Now name it (ex *custom scroll tracking*) and select `Scroll Depth` as Trigger Configuration. 
Choose if you want to measure vertical, horizontal or both scrolling and type the percentage or the pixels value, that defines the point at which the trigger fires — either at a specific percentage of the total page height or after a fixed number of pixels from the top.

The next step is to **create a new tag**, which is the measurement called we want to be triggered.
Click on `Tags > New` and name it (ex. *GA4 - custom scroll tracking*). Since everything we measure to GA4 is considered an *event*, select `Google Analytics: GA4 Event` as *Tag Configuration*, choose the [[#GA4 Configuration Tag|configuration tag]] you already set and set the name that you will see in GA4 as event name.
Under `Event Parameters` add a row and, choose a name for the parameter (like *scroll_depth*) and click on the icon near `value` field to see the predefined variable list. Click on `Built-ins` to see the complete list and select `Scroll Depth Treshold`.
Finally **select the trigger** created before, **save** and **submit**.