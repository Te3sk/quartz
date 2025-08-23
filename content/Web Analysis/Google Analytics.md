---
title: Google Analytics
date: 2025-08-11
tags:
  - analytics
  - network
category: Web Analysis
status: in_corso
author: Te3sk
description: This tool tracks and reports website traffic, user behavior, and conversions, providing a comprehensive view of performance metrics.
---
- [[#Introduction|Introduction]]
- [[#How the measurement is done|How the measurement is done]]
- [[#Basic Metrics|Basic Metrics]]
	- [[#Basic Metrics#User Identification|User Identification]]
- [[#GA4 Data Stream|GA4 Data Stream]]
	- [[#1. Open the Web Data Stream and enable Enhanced Measurement|1. Open the Web Data Stream and enable Enhanced Measurement]]
	- [[#2. Set the property's time zone and currency|2. Set the property's time zone and currency]]
	- [[#3. Define your internal traffic (IP)|3. Define your internal traffic (IP)]]
	- [[#4. Create Data Filter "Internal Traffic"|4. Create Data Filter "Internal Traffic"]]
- [[#Send a page_view at each route change|Send a page_view at each route change]]
	- [[#User Identification#Track Route Change|Track Route Change]]
	- [[#User Identification#Config Google Tag Manager|Config Google Tag Manager]]
- [[#Define Event taxonomy|Define Event taxonomy]]
- [[#Send event from the app|Send event from the app]]

## Introduction
**Google Analytics** is a web analytics service by Google that tracks and reports website or app traffic, user behavior, and conversion data.  
It’s important because it provides actionable insights into how visitors interact with your content, which marketing channels drive the most engagement, and where improvements can be made to increase performance.  
Google Analytics works by embedding a tracking code into your site that collects data on page views, events, user demographics, device types, and more.  
This data is then processed and presented in an interactive dashboard, enabling both marketers and developers to make informed, data-driven decisions to optimize user experience, content strategy, and ROI.

**Prerequisite:** Ensure that [[Google Tag Manager#Installation|Google Tag Manager]] (or another tracking implementation method) is already installed on your site so the Google Analytics tag can be deployed and start collecting data.
## How the measurement is done
GA4 is based on cookies, it process any browser as a different user with a random number and the first timestamp in which the user visits our site, these two values joined together make the **Client ID**.
We have to put the GA4 measurement code in every page of our site. That code check if there are the GA4 cookie in the browser and, if there isn't, it create one.
[introduction to server-side tagging](https://developers.google.com/tag-platform/tag-manager/server-side/intro?utm_source=advocacy&utm_medium=social&utm_campaign=gtm)
[Enhanced measurement events](https://support.google.com/analytics/answer/9216061?hl=en
## Basic Metrics
[What is a user in GA4](https://www.measurelab.co.uk/blog/users-ga4/)
### User Identification
![[GA4 - basic metrics scheme.png]]
*Users (GA4 world)* is the highest is the highest entity in we are measuring and operate with, is something closest to **devices**.
One *user* can have multiple *Session* during his interaction whit the website and during *session* there are multiple *View* or *Events* occurring.

We aren't measuring **user** ad **humans** but with something more closer to **device**

There are 4 methods of identification in GA4:
1. **User ID:** we need to implement a way to send this ID to GA4 with every hit and we need to let GA4 to know that
2. **Google Signals:** If someone with a google account give the permission, GA4 can create its own User ID
3. **Device ID:** this is the most used, the identifier is stored in the cookies
4. **Modelling:** this is the most advance, works for user who are non-consenting to be measure but google still can anonymously tracking them.
 We can choose witch method user by going to `Admin > Data Display > Reporting Identity` from our [Google Analytics Workspace](https://analytics.google.com/analytics/).
### Session & Engagement

**Session:** it's a group of users's interaction with the website. By interaction we mean *page view* and event like *adding product to cart* or *purchasing*. If not adjusted, the time window between interaction cannot be longer than 30 minutes.

**Engaged Session:** The way google define it is based on 3 conditions:
1. Lasted more than 10 seconds
2. Included conversion
3. 2 or more page views

If one or more of these conditions are satisfied, the session are mark as **engaged**.

**Engagement Rate:** the percentage of engaged sessions out of the total number of sessions.

**Bounce Rate:** The volume (percentage) of the sessions which bounced without performing any other interactions.
$$\text{Bounce Rate}=1 - \text{Engagement Rate}$$

By going to `admin > Property Settings > Data Streams > choose a datastream > Configure Tag Settings > Show More > Adjust session timeout` we can change the amount of seconds to consider a session engaged (condition 1).
In the same section, we can choose the amount of inactivity time to **consider a session expired**.


---
## GA4 Data Stream
A **Google Analytics Data Stream** is a data source that sends information from your website or app to Google Analytics 4.  
Each stream (Web, iOS, or Android) contains its own unique measurement ID, which is used in your tracking setup to route collected events and user data to the correct GA property.
##### 1. Open the Web Data Stream and enable Enhanced Measurement
In [Google Analytics Workspace](https://analytics.google.com/analytics/) click on **`admin`**$\implies$**`Data Streams`** and create a new stream by selecting **`Web Stream`**. In **`Enhanced measurement`** tab select ON and choose and choose what to track automatically (*page views, scrolling, outbound clicks, site search, videos, files*). 

If you will be sending page_views via GTM on route changes, disable “Page views” here to avoid duplicates.

##### 2. Set the property's time zone and currency
Go to **`Admin`** $\implies$ **`Property details`** and set up **Reporting time zone**
##### 3. Define your internal traffic (IP)
**Internal traffic** in GA4 refers to visits from your own team or network (e.g., office, home, VPN) that you want to exclude or mark.   By setting IP-based rules, GA4 labels these events with a `traffic_type` parameter, allowing you to filter them out so they don’t distort analytics or trigger remarketing.
Go to **`Admin`** $\implies$ **`Data Streams`** $\implies$ select your Web Stream $\implies$ **`Configure tag settings`** $\implies$ **`Show More`** $\implies$ **`Define Internal Traffic`** $\implies$ **`Create`**. Now give the rule a name, 
##### 4. Create Data Filter "Internal Traffic"
Go to **`Admin`** $\implies$ **`Data Filters`** $\implies$ **`Internal Traffic`** (select or create) $\implies$ **`Exclude`**. Set `Testing` to validate then switch to `Active` when you're sure.
## Send a page_view at each route change
The **Pageview** sends an event in the `dataLayer` at every route change without reloading. This helps to correctly measure navigation and funnels in GA4, because GMT listens for that event and fires the GA4 `page_view` tag for each new view.
#### Track Route Change
Implement a helper that acts as a hook and captures page changes in your system. This implementation depends on the technology used to build the site.
#### Config Google Tag Manager
Now you have to enable **`page_view`** in GA4 by sending a custom event (`virtual_pageview`) to GMT at each route change.
1. **GA4 Configuration Tag:** setup GA4 configuration tag without automatic sending of `page_view`
Open [GMT Workspace](https://tagmanager.google.com), click **`Tag`** $\implies$ **`New`** $\implies$ **`Tag Type:`**`Google Analytics: GA4 Configuration`, and create a GA4 configuration tag with your measurement ID (`G-XXXX`). In the option, uncheck "*Send a page view event when this configuration loads*", this avoid double counting, set the trigger to *All Page* and save.

If you have **Enhanced Measurement** $\implies$ **Pageviews enabled** in GA4, turn it off or coordinate carefully; otherwise, you'll end up with double/triple pageviews.

2. **Tell to GMT which data to read in `dataLayer`:** In GMT create 3 variable with the following names: `page_location`, `page_path` and `page_title`. They will be needed soon to fill `page_view` parameters
3. **Create `page_view` trigger:** create a ***Custom event*** trigger with **event name:** `virtual_pageview`, whenever your app pushes this event to the dataLayer, GTM will be able to react.
4. **Create the tag that send `page_view` to GA4** and connect it to the custom event `virtual_pageview`: from GMT workspace, go to **`Tags`** $\implies$ **`New`** $\implies$ **`Tag Configuration`**, now select **`Google Analytics`** $\implies$ **`GA4 Event`**. In the page that opens you have to insert your `measurement ID`, the event name (`page_view`).
Then open `Event Parameter` tab and add the 3 variables you set in step 2 as `Name: page_location; Value: {{page_location}}` (do the same for `page_path` and `page_title`). You can check all the [doc about event parameter](https://developers.google.com/analytics/devguides/collection/ga4/event-parameters?utm_source=chatgpt.com&client_type=gtag).
5. **Connect the trigger `virtual_pageview`:** in the `trigger` tab, click *Add Trigger* and select the custom event `virtual_pageview` you set in step 3
## Define Event taxonomy
**Event taxonomy** is the shared schema that defines **which events** you track, **when** they should fire, and **what names/parameters** they use (e.g., `sign_up`, `purchase`, `value`, `currency`). It ensures **consistent, high-quality data** across your app, GTM, and GA4/Ads—preventing duplicates, standardizing naming and field formats, and enabling reliable reports, funnels, and conversions.
In the [[Web Analysis#**Analytics Documentation – Structure Overview**|Analytics Documentation]], write the two list "***main events***" and "***secondary events***". The first will be the events interpreted as **conversions** and the second will be the less important events but which you still want to keep track of. Every point in the list must have 3 field: a description, the parameters needed and the trigger definition, is a best practice to add an example.
## Send event from the app
The goal of this step is to get the events to the analytics tools with correct timing, clear payload and without duplicates. 
To make it, you have to encapsulate the push in a **single dispatcher** (like a `tracker(event, params)` function) to avoid scattered and inconsistent pushes.
The events should be emitted **after the actual actions**, don't emit events on simple render/UI views if they aren't business relevant. If a flow contain more steps, send **an event for each step** and **a final event** for the result.
The name of the events must be standard, clear and stable. The parameters must have consistent type and names (*sneak_case*) and you have to add metadata for multi-step.
For **click, CTA and navigation**, choose only a way: track in the code or let the tag manager to do it, but avoid double tracking. 