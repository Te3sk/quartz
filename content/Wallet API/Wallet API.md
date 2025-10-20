---
title: Wallet API
date: 2025-10-20
tags:
  - wallet
  - api
  - back-end
  - ui
  - tools
category: Wallet API
status: in_corso
author: Te3sk
description: TODO
---
* [[Google Wallet API]]
	* [[Google Wallet API Key Concept & Terminology]]
	* [[Google Wallet API Workflow]]
* [[Apple Wallet Passes]]: TODO
# Google Wallet API
Provides a **comprehensive overview of the Google Wallet API** and how passes are issued to users. It explains the core components — such as [[Google Wallet API Key Concept & Terminology#Pass Issuer|Issuers]], [[Google Wallet API Key Concept & Terminology#Passes Class|Pass Classes]], and [[Google Wallet API Key Concept & Terminology#Passes Object|Pass Objects]] — and walks through the **standard pass development flow**, from class and object creation to [[Google Wallet API Key Concept & Terminology#JSON Web Token (JWT)|JWT]] encoding, signing, and distribution via an _Add to Google Wallet_ [[Google Wallet API Key Concept & Terminology#'Add to Google Wallet' button|button]] or [[Google Wallet API Key Concept & Terminology#'Add to Google Wallet' link|link]]. It serves as the **starting point** for understanding how the API works before diving into the technical workflow or detailed terminology.
* **[[Google Wallet API Key Concept & Terminology]]:** This document serves as a **reference glossary** for the Google Wallet API, explaining all the core entities, tools, and authentication concepts used throughout the platform. It is intended as a **foundational reference** to clarify terminology and ensure consistency across documentation and implementation.
* **[[Google Wallet API Workflow]]:** This document provides a **step-by-step implementation guide** for the Google Wallet API, demonstrating how to create, manage, and issue **Generic Passes** using the official Python client libraries.   It walks through the full workflow — from authentication and class/object creation to JWT signing and “Save to Wallet” link generation — offering a **hands-on reference** for developers integrating Google Wallet into their systems.  