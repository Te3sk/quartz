---
title: Google Wallet API
date: 2025-10-16
tags:
  - wallet
  - api
  - ui
  - back-end
category: Wallet API
status: in_corso
author: Te3sk
description: Breve descrizione del contenuto del documento.
---
# Introduction
[Official Doc](https://developers.google.com/wallet/tickets/events?authuser=1)
* **Pass Issuer:** The Passes issuer is the entity that owns the pass and is responsible for issuing passes to their customers. This could be you, the developer, or the organization that you represent. In order to become a Passes Issuer, you must first register as an Issuer.
* **event ticket class:** A `EventClass` can be thought of as scheduled event. An issuer may create multiple scheduled events with permutation of event names and timings. Each `EventClass` representing a scheduled event may contain its own appearance and data fields to meet the venue-specific requirements. In addition, `EventClass` can also be used to enable additional features like Smart Tap.
* **event ticket object:** A event ticket object is an instance of a `EventClass`. A new `EventObject` instance should be created for each customer that is distributed with a event ticket. A `EventClass` is a type of Pass Class. Pass Classes describe general information for related Pass Objects (such as style and appearance), and do not include customer details.
* **Pass Object:** A `EventObject` is a type of Pass Object. Like `EventObjects`, a Pass Object is an instance of a corresponding Pass Class. A Pass Object should be created for each customer that is issued a event ticket. Pass Objects contains customer-specific information. For example, it can be used to identify that a customer not only has a ticket for a event, but also identify which seat on the event they’ve been allotted.
* **Service account:** The service account is the identity that is used to call the Google Wallet API. Permission to access the Passes API should be granted to this service account. The **service account key** is the credential used to authenticate your application as the service account. The service account key should be considered highly sensitive and kept private. If a third party has access to the service account key, they will be able to identify themselves as the service account and perform actions that the service account is permitted to perform.
![pass class and object example](https://developers.google.com/static/wallet/images/classes-objects.svg?authuser=1)
## Adding a pass to a user's Google Wallet

To add a pass to a user's Google Wallet, you create a JSON Web Token (JWT) that contains claims you (the issuer) are making about the Passes Object instance that will be saved in the user's Google Wallet - most importantly, the Object ID of the Passes Object instance you are issuing to the user. The JWT is then delivered to the user via the a **Add to Google Wallet** button or an **Add to Google Wallet** link.
After a user clicks the button or link to add an issued pass into their Google Wallet, a link the Passes Object instance encoded in the JWT is linked to that user's Google account. This means that when the user clicks the button again, a link already exists to that Passes Object, so duplicate copies won't be added to the user's wallet.
If a user removes a pass from the Google Wallet app, the corresponding Passes Object instance is automatically de-linked from the user, but it is not deleted. This means that a user can click the **Add to Google Wallet** button or link again, to save the pass without the need for a new Passes Object instance or JWT to be created.
## Google Wallet pass development flow
[Official Doc](https://developers.google.com/wallet/tickets/events/overview/add-to-google-wallet-flow?authuser=1)
### 1. Create a Passes Class
A Passes Class defines a set of properties that are common across multiple passes, similar to a template. For example, if you were issuing tickets of an event, the Passes Class would define fields that are the same across all tickets, such as the event name, date, and time.

Every pass you issue must reference a Passes Class. You must also assign a unique ID to every Passes Class you create, which is used to reference it in when creating passes.

A Passes Class is defined in JSON format, and can be created with the Google Wallet REST API, Android SDK, or in the Google Wallet Business Console.
### 2. Create a Passes Object
A Passes Object defines the properties of a unique pass that will be issued to a specific user. For example, the Passes Object for an event ticket would define fields that are unique to a specific ticket, such as the seat number or a QR code for that ticket.

When a Passes Object is created, the Google Wallet API stores a new pass and associates it with your Issuer account. This stored pass is a combination of the unique properties of the Passes Object and the template properties of the associated Passes Class.

You must also assign each Passes Object a unique ID, which is used to reference it when issuing a pass.

A Passes Object is defined JSON format, and can be created with the Google Wallet REST API or Android SDK.
### 3. Encode the pass in a JSON Web Token (JWT)
To issue a pass to a user, a Passes Class and Passes Object must be encoded in a JSON Web Token (JWT). The JWT format is a common and open standard for representing claims between two parties. In the case of issuing passes with the Google Wallet API, JWTs are used to send a claim that a user has a right to access a specific pass that is associated with your Issuer account.

When a JWT is sent to the Google Wallet API, the encoded data is used to identify a specific pass and issue it to the user. If the pass has already been issued, this data also allows the Google Wallet API to identify that the pass is a duplicate so that it is not added to the user's Google Wallet more than once.

JWTs are defined in JSON format based on the [JWT specification](https://datatracker.ietf.org/doc/html/rfc7519). To define a JWT for issuing a pass with the Google Wallet API, you provide the information about the pass you wish to issue in the `payload` property of the JWT.
### 4. Sign the JWT with your credentials
All JWTs sent to the Google Wallet API to issue a pass must be signed with credentials you have previously provided in the Google Wallet Business Console. Signing uses your credentials to encrypt the JWT so that your passes remain secure, and to allow the Google Wallet API to authenticate that the the pass details encoded in it are valid and associated with your Issuer account.

The Google Wallet client libraries and the Android SDK provide convenience methods to sign your JWTs. There are also numerous open-source libraries available that handle the complexity of code signing for you to choose from.

For those using the Google Wallet REST API to issue passes, the JWT is signed with a Google Cloud Service Account key. For those using the Google Wallet Android SDK, the SDK automatically handles signing the JWT with the SHA-1 fingerprint of your app signing certificate.

To protect your credentials, JWTs should only be signed on your server or using the Google Wallet Android SDK in your app.
### 5. Issue the pass with an 'Add to Google Wallet' button or link
Once you have created a signed JWT, you are ready to issue your pass to a Google Wallet user! This is done by either presenting the user with a 'Add to Google Wallet' button or link. When a user clicks the button or hyperlink, the signed JWT is sent to the Google Wallet API, which then decrypts it using your saved credentials. Once the JWT signature is authenticated, the pass will be issued to the user to save in their Google Wallet.

To create a 'Add to Google Wallet' button for an Android app, use the Google Wallet Android SDK, which provides methods for generating the button. For all other platforms, including web, email, and text message, create a hyperlink in the format `https://pay.google.com/gp/v/save/<signed_jwt>`. Where possible, it is best to deliver this link to the user as a 'Add to Google Wallet' button.

For more information on using the 'Add to Google Wallet' button, see the Google Wallet API [Brand guidelines](https://developers.google.com/wallet/tickets/events/resources/brand-guidelines?authuser=1)
## Creating Passes Objects and Passes Classes in the JWT
Passes Classes and Passes Objects may be created in advance using the Google Wallet REST API or Android SDK. Once created, they are then used to issue passes by referencing their ID.

Alternatively, you may also create Passes Classes and Passes Objects 'just in time' by embedding their JSON directly in the JWT that is used to issue the pass to a user. In this method, the Passes Classes and Passes Objects are created by the Google Wallet API when the signed JWT is sent using a 'Add to Google Wallet' button or link.

For example, the following shows a JWT with a new Passes Class and Passes Object defined using the `payload.eventTicketClasses` and `payload.eventTicketObjects` properties. Notice that these properties are arrays, so they can accept one or more Passes Classes or Passes Objects. You may also specify just a new Passes Object in the JWT that references an existing Passes Class by its ID.