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
## Get Events
Events are the core of GA, they're essentially actions happening on our website or on our app by our users. Those actions are then processed and aggregated by GA and show to us in **reports**.
### Recommended Events
They are **predefined** by Google with **fixed names and parameters** (e.g., `purchase`, `login`, `search`). They aren’t automatically tracked, but Google _recommends_ you implement them because GA4 knows how to interpret and report on them in standard reports.

To add them, [[Google Tag Manager#Recommended Events|configure GMT]] and you just see them in your reports.
### Custom and DataLayer Events
TODO
## Conversions setup (Key Events)
[Create or Modify Key Events - Official Doc](https://support.google.com/analytics/answer/12844695?hl=en)
In *Universal Analytics* and in the previous version of Google analytics the conversions are called **Goals**.
In GA4, [[Google Ads#1.1 Key Metrics of Google Ads|conversions]] are events you mark as **key business actions** (like purchases or sign-ups); they are important because they measure goal achievement and are used to optimize reports and linked ad platforms (e.g. [[Google Ads]]).
There is a set of events which are by default created and mark as *conversion* (eg. `first visit` or `purchease`). 
You can set them by going to `Admin > Data Display > Events`. Now you will see 2 lists: `Events` that contain all the events you collected and `Key Events` that aren't already set up.
You can create and modify events in Google Analytics. Modifying an event is a way of changing an existing event so it measures what you want it to measure. Creating an event copies over an existing event so you can measure what you want to measure without changing the original event.
### Purchase - Default Key Event
The `Purchase` event is the most striking conversion event, so GA4 automatically get it as a **key event** and you can't unmark it. 
If you want, you can [[#Key Event Value|change the event value]].
### Set Existing Event as Key Event
By going to `Admin > Data Display > Events` and selecting  `Recent Events` list, you can see all the 100 most recent events your GA4 has received. To mark one of them as **key event**, just click on the **star icon on the left** in the row of the event you choose.
If you want, you can [[#Key Event Value|change the event value]].
### Key Event Value
In Google Analytics key events, the **`value` parameter** represents the numerical worth associated with an interaction, such as the total price of a purchase or the monetary equivalent of a conversion. It is important because it allows GA4 to measure not just *how often* events happen, but also their **business impact**, enabling accurate revenue reporting, ROI calculations, and ad optimization. By assigning meaningful values, you ensure that analytics data reflects real outcomes rather than just user activity.
By default, GA4 get the `value` parameter of the event as its **economic value**. Depending on the [[|parameters you send with the purchase event (TODO - ADD LINK TO THE RIGTH SECTION)]], you can modify the value by going to `Admin > Data Display > Events > 3 dot on Purchase > Set default key event value`.
