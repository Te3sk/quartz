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
## 1. Introduction
**Google Tag Manager (GTM)** is a free tag management system by Google that allows you to add, update, and manage tracking codes (tags) on your website or app without directly modifying the source code.  
It’s important because it centralizes all your [[Google Analytics|analytics]], [[Google Ads|advertising]], and custom tracking scripts in one interface, reducing deployment time and minimizing the risk of errors from manual code edits.  
GTM works by embedding a small container script in your site’s HTML; inside the GTM dashboard, you define tags (e.g., Google Analytics, Meta Pixel), triggers that decide when those tags fire (e.g., page load, button click), and variables that store dynamic values.  
This approach enables marketers and developers to deploy and adjust tracking implementations quickly, without requiring a full development cycle for every change.
## 2. How GMT Works
There are **3 main instances:**
* **Tags:** a measurement code
* **Triggers:** the conditions when we want to fire measurement codes
* **Variables:** any data we can collect from the website or push in the Data Layer and collect to the tags
## 3. Setup
[Google press official releases regarding GA4](https://support.google.com/analytics/answer/9164320#071122&zippy=%2Creleases)
### 3.1. Create and Configure Account
To start using **Google Tag Manager (GTM)**, you first need to create an account and set up a container:
1. Go to [Google Tag Manager](https://tagmanager.google.com) and log in with your company Google account.  
2. Click on **Create Account**.  
	- Enter the **Account Name** (e.g., Digital On).  
	- Select the **Country** where your business operates.  
3. Create a **Container** for your website:  
	- Enter the **Container Name** (usually your website domain, e.g., `digitalon.com`).  
	- Choose the **Target Platform** → select **Web** (unless you are configuring for iOS, Android, or a server container).  
4. Click **Create** and accept the Terms of Service.  

Once the account and container are created, GTM will provide you with the installation code snippets for your website. These snippets must be added before you can start configuring tags, triggers, and variables.
###  3.2. Install the GMT Manager Code
First you have to create the **connection between GMT and your website**. To do that, in [GMT Workspace](https://tagmanager.google.com) **create a container** and then click `Admin > Install Google Tag Manager`. There  you can find 2 HTML tag and you have to paste them in the HTML files of your website (each page), one in the `<head>` and the other in the `<body>`.
### 3.3. GA4 Configuration Tag
Now click on `New Tag`, name it *GA4 - Configuration Tag* and in `Tag Configuration` choose `Google Analytics: GA4 Configuration`. Now you have to paste the `Measurement ID` from [[Google Analytics#GA4 Data Stream|Data Stream Details]] in your [Google Analytics Workspace](https://analytics.google.com/analytics/). 
Now you have to select a **Trigger**, or when you want to fire the measurement. There are couple of predefined triggers, if you want to measure **all the website** click on `All Pages`. 
Click on `Save` and then to `Submit` to make the changes work.

Depending on the version of GMT, `Google Analytics: GA4 Configuration` may be **deprecated** and you wont find it. In this case, choose `Google Tag` as **Tag Type**. 
Now, in [Google Analytics Workspace](https://analytics.google.com/analytics/), go to `Admin > Data Stream > [your data stream] > Configure Tag Settings`; here, under *Your Google Tag*, click the Google tag (left side of diagram) and copy the **Google Tag ID** under *Tag Details*.
Return to the GMT tab and paste the **Google Tag ID** in the file that requires it.
As **trigger** choose `Initialization - All Pages`, in this way the tag will be loaded before the others tags (pageview or DOM).
### 3.4. Events Configuration
There are way to **send event information** to GA4 by GMT:
* **Recommended Events:** those events are know by GMT, so you can choose the one you need from a list and easily set them up
* **Custom Events:** are events that GMT doesn't know and that are specific to your business. In this case you need a little more steps to set them up.
* **Push in DataLayer:** you have to use this way for the most critical events of your system. With *critical events* we mean events that are particularly important for analyzing your business and that need more specific configuration. Those events are pushed from the code of your website.
#### 3.4.1. Recommended Events
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
#### 3.4.2. Custom Events
It's an event that is specific to your business and GA doesn't know anything about it yet. There are a few extra steps you need to do to make the datas available in your report.
1. **Variable:** 
	Choose which property you want to use to identify the component (class, id, ...), then in [GMT Workspace](https://tagmanager.google.com) go to `Variables > Configure` and select the property you chosen in the *built-in variables* list that opens.
2. **Trigger:**
	Go to `Trigger > New` to create the trigger for this event. Choose a recognizable **name** and the right **Tag Type** from the list. Then use the additional settings (them depend on the tag type) to identify the component that trigger the event (id, className, ...).
3. **Tag:**
	Now go to `Tag > New` to create the tag for this event.  After you name it, choose `Google Analytics: GA4 Event` as **Tag Type**, select the right **configuration tag** or insert you **Measurement ID** and **name your event**  as you like (best practice is to use lowercase and to use `_` instead of spaces). Finally, set up **parameters** if you need, the best practice is to use them identify the component that trigger the event or identify the case/user-flow of the event.

Once you submit the new version, you will see the new events in GA4 reports.
#### 3.4.3. DataLayer
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
##### 3.4.3.1. Set up
To enable DataLayer pushing, you need to add some code before [[#Install the GMT Manager Code|GMT Manager Code]] like that:
```html {1-3, 12-17}
<script>
window.dataLayer = window.dataLayer || [];
</script>
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src= 'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-XXXXXX');</script>
<!-- End Google Tag Manager -->
```
##### 3.4.3.2. Event Handlers
To push an event in the dataLayer, just use the built-in function:
```js
datalayer.push({"parameter-name" : "parameter-value"})
```
The good practice is to have `"event_name" : "[name]"` as first parameters. 
The specific implementation depends on which technology you used to build the site.
##### 3.4.3.3. Send Events to GA4
To send event from dataLayer to [[Google Analytics|GA4]], you have to manually create a tag for each event:
1. **Create Variables:** Go to `Variables > User-Defined Variables > New`, now give a simple and mnemonic name to the variable, choose `Data Layer Variable` as **Variable Type** and type in `Data Layer Variable Name` exactly as it appears in the datalayer. Do it for each event parameter you want to track.
	If datalayer event contains arrays or other dynamic datas, you can use [[#Custom JavaScript Variables]].
2. **Create Trigger:** Go to `Trigger > New`, choose `Custom Event` as **Trigger Type** and type the `Event Name` exactly as it appears in the datalayer object.
3. **Create Tag:** Go to `Tag > New`, choose `Google Analytics: GA4 Event` as **Tag Type** and insert the Measurement ID of your [[Google Analytics|GA4]]. Now choose the `Event Name` you will see in your GA4 reports and insert in `Event Parameters` all the variable you want to track and you set before. Set the trigger you just create as trigger and save.
### 3.5. Custom JavaScript Variables
**Custom JavaScript Variables (CJSV)** in Google Tag Manager allow you to extend GTM’s functionality by writing small JavaScript functions that dynamically return values.  
They are especially useful when built-in variables are not enough or when you need to apply logic that adapts to specific conditions on your website or app.
A Custom JavaScript Variable is essentially a JavaScript function that **must return a value**.  
This value can be a string, number, boolean, array, or object, and GTM can then use it in tags, triggers, or even as input to other variables.  
Each time the variable is referenced, GTM executes the function and retrieves the returned value.
**Basic structure:**
```javascript
function() {
  return "Hello World";
}
```
**Common Applications**
- **Transform values**: format or normalize data (e.g., lowercase URLs, format dates).
- **Conditional logic**: return different values depending on user state, device type, or page.
- **Extract data**: pull dynamic information from the DOM, cookies, or query parameters.
- **Combine variables**: merge values from multiple GTM variables into one.
- **Fallbacks**: provide default values when other variables are missing or undefined.
**Best Practices:**
- Keep functions **short, simple, and focused** on returning a value.
- Always include a **fallback return** to avoid breaking triggers when expected data is missing.
- Add comments explaining what the variable does and when it is used.
- Avoid heavy logic or loops that may slow down page performance.
- Minimize direct DOM dependencies; if the page structure changes, the variable should still fail gracefully.
- Test in **GTM Preview Mode** and use `console.log` for debugging before publishing.
**Limitations:**
- CJSV run **client-side only**; they cannot access external libraries or APIs directly.
- They are evaluated **on demand** when GTM calls them, not continuously in the background.
- Complex logic is better handled in your application code or with dedicated scripts rather than inside GTM.
### 3.6. Scroll Tracking - Advanced Measurement Example 
First you have to create a **trigger**, to rule when you want to send the datas to GA4, by clicking on `Triggers > New`. Now name it (ex *custom scroll tracking*) and select `Scroll Depth` as Trigger Configuration. 
Choose if you want to measure vertical, horizontal or both scrolling and type the percentage or the pixels value, that defines the point at which the trigger fires — either at a specific percentage of the total page height or after a fixed number of pixels from the top.

The next step is to **create a new tag**, which is the measurement called we want to be triggered.
Click on `Tags > New` and name it (ex. *GA4 - custom scroll tracking*). Since everything we measure to GA4 is considered an *event*, select `Google Analytics: GA4 Event` as *Tag Configuration*, choose the [[#GA4 Configuration Tag|configuration tag]] you already set and set the name that you will see in GA4 as event name.
Under `Event Parameters` add a row and, choose a name for the parameter (like *scroll_depth*) and click on the icon near `value` field to see the predefined variable list. Click on `Built-ins` to see the complete list and select `Scroll Depth Treshold`.
Finally **select the trigger** created before, **save** and **submit**.
## 4. Track GHL iFrame forms with GTM
[Guide link](https://karolkrajcir.com/post/how-to-track-go-high-level-iframe-forms-with-google-tag-manager)
When a form is embedded via an **iframe**, it runs in an isolated browsing context. This means the main website cannot directly access user interactions inside the iframe, and Google Tag Manager cannot “see” form submissions by default. To track these events, the iframe must actively communicate with the parent page (e.g., via `postMessage`), allowing the parent GTM container to capture the submission as if it were happening on the main site.
### 4.1. Create and setup a new container in the GTM Account
Create a new container in the same GTM account where you have the main container of your website:
1. Go to `Admin`$\rightarrow$`+ Create Container`
2. Enter a descriptive name and select `Web` as target platform
3. Copy the GTM installation code and insert it in the iFrame
	This step implementation depend on how you create the iFrame. If you just code it just paste the code in the header, if you built it with some tools (like GHL) just put a HTML custom block as first and paste the code inside
### 4.2. PostMessage to Parent Domain
The `postMessage` API allows an iframe to securely send data to its parent domain, even though they run in separate browsing contexts. We use it here so the form iframe can notify the main website about a submission, enabling the parent GTM container to capture and track the event.
First find a way to trigger the form submission event, then create a new **Custom HTML Tag** with the following code:
```html
<script>
  try {
    var postObject = JSON.stringify({
      event: "[your event name]",
    });
    parent.postMessage(postObject, "https://[your domain]/");
  } catch (e) {
    window.console && window.console.log(e);
  }
</script>
```
Insert `[your event name]` and `[your domain]` where indicated.
### 4.3. Listener from Parent Container
The event listener in the parent container waits for messages sent from the iframe via `postMessage`. When a submission occurs, it captures the message, pushes a custom event into the parent’s `dataLayer`, and allows GTM on the main site to track the action as if it happened directly on the page.
To make that, just go to the main container of the site and create new **Custom HTML Tag**. Select `All Pages` as trigger and paste the following code:
```html
<script>
  (function () {
    window.dataLayer = window.dataLayer || [];
    function onMessage(e) {
      // accept messages only from the domain of your iFrame
      if (e.origin !== "[iFrame Domain]") return;
      try {
        // check if the iFrame sends object
        var payload = (typeof e.data === "string") ? JSON.parse(e.data) : e.data;
        // check which event are sended by the iFrame and push in the dataLayer
        if (payload && payload.event === "[your event name]") {
          window.dataLayer.push({
            event: "[the event you want to track in dataLayer]"
          });
        }
      } catch (err) {
        console.log("Listener error:", err);
      }
    }
    window.addEventListener("message", onMessage, false);
  })();
</script>
```
* Insert `[iFrame Domain]`: the domain where your iFrame is hosted
* Insert `[your event name]`: the **same name** you used in the [[#4.2. PostMessage to Parent Domain|previous step]]
* Insert `[the event you want to track in dataLayer]`: how you want to call the event in the dataLayer. This isn't the final name you will see in [[Google Analytics|GA4]], is a **good practice** to maintain the same unique name in all the steps.
### 4.4. Track the event
Now the final step: the [[#4.3. Listener from Parent Container|listener]] wait for the event and push it in the data layer, you have just to [[#3.4.3.3. Send Events to GA4|send a data layer event to GA4]] as indicated before in this page.
