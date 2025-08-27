---
title: Generic Workflow to implement Analytics on a website
date: 2025-08-27
tags:
  - analytics
  - network
  - workflow
category: Web Analysis
status: in_corso
author: Te3sk
description: This file explain how to implement Analytics products in a website, regardless of the technology used to build the site
---
## Step List
1. Write the [[Web Analysis#**Analytics Documentation – Structure Overview**|Analytics Documentation]] of your project
2. Create and configure [[Google Tag Manager#Create and Configure Account|Google Tag Manager]] and [[Google Analytics#Create and configure the Account|Google Analytics]] Accounts
3. Setup [[Google Analytics#Basic setup - Data stream|GA4 Data Stream]]
4. Install [[Google Analytics#Hardcoded measurement - Tag instructions|GA4 Hardcoded measurement]] in your project
5. [[Google Tag Manager#Install the GMT Manager Code|Install the GMT Manager Code]]
6. Create a [[Google Tag Manager#GA4 Configuration Tag|GA4 Configuration Tag]] in GMT
7. Do the [[Google Analytics#Additional Setup|Additional Setup]] of GA4 if needed
8. Set up [[Google Tag Manager#Set up|DataLayer]] if needed
9. Set up the [[Google Tag Manager#Events Configuration|configuration to send events]]
	* Send critical events by [[Google Tag Manager#Event Handlers|pushing them in the dataLayer]]
	* Send non-critical events using [[Google Tag Manager#Recommended Events|Recommended Events]] and [[Google Tag Manager#Custom Events|Custom Events]]