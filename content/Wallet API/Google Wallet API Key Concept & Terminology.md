---
title: Key Concept & Terminology
date: 2025-10-17
tags:
  - wallet
  - back-end
  - api
  - ui
category: Wallet API
status: completed
author: Te3sk
description: Key concept and terminology related to Google Wallet API
---
### Passes
#### Pass
A pass is an instance of a Passes Object that is issued to a user to save in their Google Wallet. The Google Wallet API provides support for a number of common pass types, including boarding passes, event tickets, ID cards, and more. The Google Wallet API also provides a Generic pass type that can be used to create passes that are not otherwise specifically supported.
In most cases, a pass is created using both a Passes Class and a Passes Object.
#### Pass Issuer
A Pass Issuer is an entity that creates passes and issues them to users to save in their Google Wallet. The Pass Issuer owns the passes, and can create, issue, and update them. Pass Issuers can be individual developers, companies and organizations, as well as aggregators who create and issue passes on behalf of others, such as a company that specializes in implementing tickets for events or coupons for retailers.
#### Passes Class
A Passes Class can be thought of as a shared template that passes are created from. A Passes Class defines certain properties that will be included in all passes that use it. A Pass Issuer can create multiple classes, each with their own distinctive set of properties that define attributes like style and appearance, as well as additional features like Smart Tap, and the Enrollment and Sign In.
In most cases, a pass is created by providing a Passes Class to define the template of the pass, and a Passes Object to define the specifics of the individual pass being issued.
#### Passes Object
A Passes Object defines an individual pass that is issued to a user to save in their Google Wallet. Passes Objects often contain user-specific information. For example, while a Passes Class might define what a gift card pass for a specific shop will look like, the Passes Object will provide specific details like the balance or expiration date.
A Passes Object must be created for every pass that is issued, as opposed to Passes Classes which can be shared across many pass instances.
#### Private passes
Some passes you create with the Google Wallet API may contain sensitive user data. These passes require extra protection to help keep your users data safe and are managed differently in the Google Wallet API by using the [Generic Private pass](https://developers.google.com/wallet/generic-private-pass) type. The Generic Private pass should be used in cases where sensitive data (as defined in the [Google Wallet API Acceptable Use Policy](https://payments.developers.google.com/terms/aup)) is included in your pass, and may be subject to additional privacy controls and review during onboarding.
#### Smart Tap
[Smart Tap](https://developers.google.com/wallet/smart-tap) is a Google proprietary near-field communication protocol for conveying data between a mobile device and an NFC terminal. Smart Tap technology lets users redeem passes saved in their Google Wallet by holding their phone close to any compatible NFC terminal.
To use the Smart Tap protocol with your passes, you must establish a relationship with a [Smart Tap capable terminal provider](https://developers.google.com/wallet/smart-tap/introduction/overview#smart_tap_capable_terminals).
### APIs and SDK
#### Google Wallet API
The Google Wallet API is a service provided by Google that allows you create and issue passes for users to save in their Google Wallets. The API can be used in several different ways, including the Google Wallet REST API, the Google Wallet Android SDK, and the Google Wallet console.
#### Google Wallet REST API
The Google Wallet REST API is an interface for creating and managing passes programmatically by sending HTTP requests to the Google Wallet API.
To use the Google Wallet REST API, you'll also need a Google Cloud account to create a service account, which is used to authenticate requests to the Google Wallet REST API.
#### Google Wallet Android SDK
The Google Wallet Android SDK provides a set of convenience methods for working with the Google Wallet API in your Android apps, such as creating and issuing passes.
### Add to Google Wallet
#### 'Add to Google Wallet' button
The 'Add to Google Wallet' button is a Google-approved asset for presenting a pass to a user. When a user clicks or taps the button, an 'Add to Google Wallet' link should be triggered to start the flow of adding the issued pass to the user's Google Wallet.
It is recommended that you use the 'Add to Google Wallet' button whenever possible, since it's a familiar UI element that your users already know how to interact with.
Assets and guidelines for using the button are available in the [Google Wallet API Brand Guidelines](https://developers.google.com/wallet/generic/resources/brand-guidelines).
#### 'Add to Google Wallet' link
With an 'Add to Google Wallet' link, you can issue a pass to a user with a normal hyperlink. This can be used anywhere you are able to use hyperlinks, such as email, SMS, websites, and mobile apps.
'Add to Google Wallet' links are created by appending a signed JWT to the URL `https://pay.google.com/gp/v/save/`.
### Issuer account
#### Demo Mode
When you create your Issuer account, it will be in 'Demo Mode' until you are approved for publishing access. In demo mode you can create passes, but you can only issue them to users with the 'Admin' or 'Developer' roles of your Issuer account, or user's that have been added as test accounts in the Google Wallet console.
While in 'Demo Mode', the title of any passes you issue will automatically begin with the words '[TEST ONLY]' to indicate that the pass is for testing purposes only.
#### Test accounts
When your Issuer account is in 'Demo Mode', if you want to issue passes to any user that doesn't have the 'Admin' or 'Developer' roles for your account, you must add them as test accounts in the Google Wallet console. Users enrolled as test accounts will be able to add passes issued by you to their Google Wallet. This is useful for testing your passes with a wider audience while in 'Demo Mode'.
#### Business profile
To create an Issuer account for the Google Wallet API, you must set up a business profile when you register for the Google Pay & Wallet console. A business profile provides Google with basic information about your company or organization, and is required to be approved for publishing access.
#### Publishing access
Before you can issue passes that any user can save to their Google Wallet, you must be approved for publishing access. To be approved for publishing access, you must have created at least one Passes Class, and have a complete business profile. Issuers that want to issue passes using the Google Wallet Android SDK must also submit the SHA-1 fingerprint for their app.
To request publishing access, go to the Google Wallet console and click the 'Request Publishing Access' button. The Google Wallet team will review your request and notify you once you've been granted publishing access.
### Authentication
#### JSON Web Token (JWT)
JSON Web Tokens are a commonly used industry standard for securely transferring information as a JSON object. When using the Google Wallet API, you encode the details of the Passes Object you want to use to create a pass instance in JWT (pronounced "jot") format, then send that JWT in a request to the Google Wallet API.
JWTs are kept secure by signing them with a shared secret before they are sent to the Google Wallet API. If you are using the Google Wallet REST API, the signing secret is your Google Cloud service account key. If you are using the Google Wallet Android SDK, the signing secret is the SHA-1 fingerprint for your Android app.
#### Service account
A [Google Cloud service account](https://cloud.google.com/iam/docs/service-account-overview) is a special kind of account typically used by an application or compute workload, rather than a person. In the case of the Google Wallet API, a service account is what you will use to authenticate requests sent to the Google Wallet REST API.
Service accounts are created in the [Google Cloud console](https://console.cloud.google.com/iam-admin/serviceaccounts). To use a service account, you will also need to [enable the Google Wallet API in the Cloud console](https://console.cloud.google.com/marketplace/product/google/walletobjects.googleapis.com) to allow the service account to make requests to the Google Wallet REST API.
#### Service account key
A service account key is the credential you will use to authenticate calls to the Google Wallet REST API. The service account key is considered highly sensitive and must be kept private, as it grants access to many of the Pass Issuer features of your account using the Google Wallet REST API, including creating Passes Classes and Passes Objects.
#### SHA-1 fingerprint
The SHA-1 fingerprint of your Android app signing certificate is the credential you will use to autheticate calls to the Google Wallet API when you use the Google Wallet Android SDK. The SHA-1 fingerprint of your certificate is generated using either Gradle or keytool. To use the fingerprint to autheticate your requests, you must register it in the Google Wallet console.