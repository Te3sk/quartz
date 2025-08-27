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
- [[#How GA4 Works|How GA4 Works]]
	- [[#How GA4 Works#How the measurement is done|How the measurement is done]]
	- [[#How GA4 Works#Basic Metrics|Basic Metrics]]
		- [[#Basic Metrics#User Identification|User Identification]]
		- [[#Basic Metrics#Session & Engagement|Session & Engagement]]
		- [[#Basic Metrics#Users|Users]]
		- [[#Basic Metrics#Time measurement|Time measurement]]
- [[#Setup account|Setup account]]
	- [[#Setup account#Basic setup - Data stream|Basic setup - Data stream]]
	- [[#Setup account#Hardcoded measurement - Tag instructions|Hardcoded measurement - Tag instructions]]
- [[#Additional Setup|Additional Setup]]
	- [[#Additional Setup#Data Retention|Data Retention]]
	- [[#Additional Setup#Data Stream Tag Setting|Data Stream Tag Setting]]
	- [[#Additional Setup#Modify events|Modify events]]
- [[#Conversions setup|Conversions setup]]
- [[#--------- OLD ---------|--------- OLD ---------]]
- [[#GA4 Data Stream|GA4 Data Stream]]
	- [[#Time measurement#1. Open the Web Data Stream and enable Enhanced Measurement|1. Open the Web Data Stream and enable Enhanced Measurement]]
	- [[#Time measurement#2. Set the property's time zone and currency|2. Set the property's time zone and currency]]
	- [[#Time measurement#3. Define your internal traffic (IP)|3. Define your internal traffic (IP)]]
	- [[#Time measurement#4. Create Data Filter "Internal Traffic"|4. Create Data Filter "Internal Traffic"]]
- [[#Send a page_view at each route change|Send a page_view at each route change]]
	- [[#Modify events#Track Route Change|Track Route Change]]
	- [[#Modify events#Config Google Tag Manager|Config Google Tag Manager]]
- [[#Define Event taxonomy|Define Event taxonomy]]
- [[#Send event from the app|Send event from the app]]
## Introduction
**Google Analytics** is a web analytics service by Google that tracks and reports website or app traffic, user behavior, and conversion data.  
It’s important because it provides actionable insights into how visitors interact with your content, which marketing channels drive the most engagement, and where improvements can be made to increase performance.  
Google Analytics works by embedding a tracking code into your site that collects data on page views, events, user demographics, device types, and more.  
This data is then processed and presented in an interactive dashboard, enabling both marketers and developers to make informed, data-driven decisions to optimize user experience, content strategy, and ROI.

**Prerequisite:** Ensure that [[Google Tag Manager#Installation|Google Tag Manager]] (or another tracking implementation method) is already installed on your site so the Google Analytics tag can be deployed and start collecting data.

You can try the features on the [GA4 Demo Account](https://support.google.com/analytics/answer/6367342#access&zippy=%2Cin-this-article).
## How GA4 Works
### How the measurement is done
GA4 is based on cookies, it process any browser as a different user with a random number and the first timestamp in which the user visits our site, these two values joined together make the **Client ID**.
We have to put the GA4 measurement code in every page of our site. That code check if there are the GA4 cookie in the browser and, if there isn't, it create one.
[introduction to server-side tagging](https://developers.google.com/tag-platform/tag-manager/server-side/intro?utm_source=advocacy&utm_medium=social&utm_campaign=gtm)
[Enhanced measurement events](https://support.google.com/analytics/answer/9216061?hl=en)
### Basic Metrics
[What is a user in GA4](https://www.measurelab.co.uk/blog/users-ga4/)
#### User Identification
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
#### Session & Engagement
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
#### Users
**Active Users:** are the users who had at least one **engaged sessions**. There always more **Total Users** than the **Active Users**. 
#### Time measurement
 The time are measured by the events timestamp (pageview or others), then GA4 calculate how much time passes between one event and another. In the example below we can see that GA4 get 0 minutes of `Page 4` viewing because the exit isn't an event and doesn't send a timestamp.
![[GA4 - timing scheme.jpg]]
The problem is that this system does not consider the option where a user visits other sites between one event and another on our site, which is why [[Google Analytics#Session & Engagement|engagement]] is a much more used and reliable metric.
The problem was solved by adding the **Unload Event**, that could be the unfocus or the closure of our website. With this event we can get a report closer to reality and also know how many time the user spent on the last page.

## Setup account
### Create and configure the Account
**TODO**
### Basic setup - Data stream
The first thing to do is to **create the account**. To do that, login into google with any GA4 account and click on `Admin > Create Property`, there  insert required informations. Then select the platform type (`Web`, `Android App` or `IOS App`), setup the relative **data stream** with the required informations, for web are `stream name` and `site URL`, and the **enhanced measurement**, the possibility to measure some interactions and content in addition of standard `pageview`.
### Hardcoded measurement - Tag instructions
Now you can see `Web Stream Details` (if you choose `web` as platform), with the details of the data stream you just created. There you can find `Tag Instructions`, by selecting it you can connect your website to GA4. The setup page will open and you have 2 way to make the connection:
* **Install with a website builder:** if your builder is in the list, you can automatically connect the system by selecting the correct builder
* **Install manually:** you will see the **HTML Google Tag** and you have to copy and paste in your website on each page, ideally as high as possible in the HTML code. This is the moment from which you are starting to collecting the datas, it works from the moment you copy that to your website (and deploy). 
## Additional Setup
### Data Retention
By going to `Admin > Data Settings > Data Retention` you can increase the retention of the data from 2 to 14 months. In this way you will have a lot more datas you can then aggregate.
### Data Stream Tag Setting
By going to `Admin > Data Stream > [your data stream] > Configure tag settings` you can access 2 useful features:
* **`List of unwanted referral`:** you can define the list of unwanted referrals defines domains that should be excluded from referral traffic in GA4, preventing sessions from being attributed to those sources (e.g. payment gateways) instead of the original traffic source.
* **`Adjust session timeout`:** you can edit the timing values for the [[#Session & Engagement|session timeout]] and for the [[#Session & Engagement|engaged session]]. Adjusting the timer for engaged sessions lets you define how long a user must stay active (e.g. 10s vs 30s) before GA4 counts it as engagement, which affects metrics like bounce rate and engagement rate.
### Modify events
By going to `Admin > Data Stream > Modify Events` you can adjust event parameters or rename events directly in GA4, so data is cleaned or standardized before being processed in reports. Here you can create rules that rename events or change their parameters by setting conditions (e.g. when event name = X, change it to Y), so GA4 processes and reports the adjusted version instead of the original.
## Conversions setup
In *Universal Analytics* and in the previous version of Google analytics the conversions are called **Goals**.
In GA4, conversions are events you mark as **key business actions** (like purchases or sign-ups); they are important because they measure goal achievement and are used to optimize reports and linked ad platforms (e.g. [[Google Ads]]).
There is a set of events which are by default created and mark as *conversion* (eg. `first visit` or `purchease`). You can see them by going to `Configure > Event` or `> Conversions`, there you can check and uncheck them as conversion or even add them.
You can also **create a new event** by setting up conditions based on existing events. For example, if we want to track as conversion the viewing of the *thank-you-page*, we can set a double condition: `event_name - equals - page_view` and `page_location - contains - [thank you page]`. Then go to *conversion* tab and add the event you just created.
## Get Events
Events are the core of GA, they're essentially actions happening on our website or on our app by our users. Those actions are then processed and aggregated by GA and show to us in **reports**.
### Recommended Events
They are **predefined** by Google with **fixed names and parameters** (e.g., `purchase`, `login`, `search`). They aren’t automatically tracked, but Google _recommends_ you implement them because GA4 knows how to interpret and report on them in standard reports.

To add them, [[Google Tag Manager#Recommended Events|configure GMT]] and you just see them in your reports.

## --------- OLD ---------
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