---
title: Apple Wallet Passes
date: 2025-10-21
tags:
  - wallet
  - api
  - back-end
  - ui
category: Wallet API
status: in_corso
author: Te3sk
description: Guide to generate Apple Wallet Passes with the official Apple API
---
- [[Apple Wallet Passes Workflow]]
# Creating the Source for a Pass
The **source directory** of an Apple Wallet pass contains all the assets and metadata the system needs to display it. In a membership-based system—where each user receives a personalized, static pass identified by a unique QR code—the pass source serves as the foundation for automated generation scripts.
## Structure Overview
Each pass is defined within a **directory** named after the final pass followed by the `.pass` extension  
(e.g., `Member123.pass`).  
This directory includes:
- **Image files** for the icon, logo, and optional backgrounds.
- A **`pass.json`** file containing metadata, field definitions, and display strings ([Pass Object](https://developer.apple.com/documentation/walletpasses/pass))
- Optional **localization folders** (`.lproj`) for language-specific text and images. 
The `.pass` directory is later signed and zipped into a `.pkpass` file for distribution.
## Required and Optional Files
**Mandatory files:**
- `pass.json` — defines all information displayed on the pass and associated metadata.
- `icon.png` (and `icon@2x.png`, `icon@3x.png`) — used for notifications, email attachments, and the lock screen.
**Optional files:**
- Other visual assets depending on pass type (e.g., `logo.png`, `background.png`).
- Localization directories containing language-specific resources.
Example structure:
```bash
simple.pass/
├── icon.png
├── icon@2x.png
├── thumbnail.png
├── thumbnail@2x.png
├── pass.json
├── en.lproj/
│   ├── logo.png
│   ├── logo@2x.png
│   └── pass.strings
└── zh-Hans.lproj/
    ├── logo.png
    ├── logo@2x.png
    └── pass.strings
```
## Localization (Optional)
To **support multiple languages**, create a subdirectory for each locale using the format  
`[language]-[region].lproj` (e.g., `en.lproj`, `zh-Hans.lproj`).
Each folder may contain:
- Localized images (`logo.png`, `logo@2x.png`, etc.).
- A `pass.strings` file defining localized text.
If a global (non-localized) version of an image exists at the top level, it overrides localized copies. Localized images might not refresh automatically once a pass has been added to Wallet.
## Localizing Text Fields
Apple Wallet automatically localizes **standard date, time, and currency fields** defined in ISO 8601 or system-recognized formats within `pass.json`.

Custom text fields, however, require manual localization through key-value pairs.
**Process:**
1. In `pass.json`, assign a key name instead of a literal string to each text field:
```json
"primaryFields": [
	{ 
		"key": "offer", 
		"value": "OfferAmount", 
		"label": "OfferAmountLabel" 
	}
]
```
2. In each localization folder, define these keys in `pass.strings`:
	- `en.lproj/pass.strings`
```text
"OfferAmount" = "100% off";
"OfferAmountLabel" = "Anything you want!";
```
	- `zh-Hans.lproj/pass.strings`
```text
"OfferAmount" = "100% 折扣";
"OfferAmountLabel" = "尽享所需一切！";
```
Non-ASCII text in `.strings` files must use **UTF-16 encoding**.
## Implementation Context (Membership Automation)
In an automated membership system, each user’s `.pass` directory can be dynamically generated via Python scripts using individualized data—such as name, membership ID, and QR code. Since membership information remains static, these values are embedded directly in `pass.json` and images, requiring no real-time updates once distributed.

The output of each generation process is a complete `.pass` folder ready to be signed and packaged into a `.pkpass` bundle for multi-device installation.
# Building a Pass
A distributable Apple Wallet pass is a **signed bundle** that contains its JSON definition, visual assets, and optional localizations.  
In a membership-based system—where each user receives a unique, static pass (with a personal QR code and immutable information)—the signing and bundling steps ensure that every generated `.pkpass` file is valid, trusted, and installable across multiple user devices.

To build a valid pass, the process consists of five main stages:
1. Prepare the source files for the pass (see [[Apple Wallet Passes#Creating the Source for a Pass|Creating the Source for a Pass]]).
2. Create a **Pass Type Identifier** linked to your organization.
3. Generate a **signing certificate** associated with that identifier.
4. Create a **digital signature** using that certificate.
5. Package all files into a **signed bundle** (`.pkpass`).

The signed bundle ensures that each pass can be verified by Apple Wallet and uniquely associated with its issuer.
## Pass Type Identifier
Each category of passes (e.g., event tickets, membership cards) requires a **unique pass type identifier**, written in **reverse-DNS format**, such as:
```text
com.example-company.membership.card
```
This identifier groups related passes under the same issuer.  
Individual passes are then distinguished by their **serial number**, which must be unique per user.  
If a pass with the same identifier and serial number already exists on a device, it is replaced automatically when reissued.

In practice, you define:
- `passTypeIdentifier` → your organization’s registered Pass Type ID.
- `serialNumber` → a unique per-user code, generated dynamically by your automation script.
## Create a Signing Certificate
Each pass must be **digitally signed** using a valid **Pass Type ID certificate** issued through the Apple Developer Program.
- A certificate signing request (CSR) is required to obtain the certificate.
- Once generated and downloaded, the certificate and its private key are used to sign the pass data.
- Only passes signed with a valid certificate matching the declared `passTypeIdentifier` and `teamIdentifier` can be added to Apple Wallet.

This certificate must be installed on the machine or environment that performs automated pass generation.
For more information on signing in to your account and creating identifiers, see [Developer Account Help](https://developer.apple.com/help/account/).
## Signing and Packaging the Pass
The signing and bundling process transforms the `.pass` source folder into the final `.pkpass` file.  
Signing a pass requires a signing certificate for the pass type identifier. Before you can generate a signing certificate you need a certificate signing request (CSR). To learn how to generate a CSR, see [Create a certificate signing request](https://developer.apple.com/help/account/create-certificates/create-a-certificate-signing-request).
The steps can be fully automated in your backend or Python scripts.

For more information on signing into your account and creating signing certificates, see [Developer Account Help](https://developer.apple.com/help/account/).

Key operations include:
- **Manifest generation:**  
    Create a `manifest.json` file at the top level of the pass directory.  
    This file maps every included asset to its SHA1 hash.  
    Example:
```json
{
  "icon.png": "2a1625e1e1b3b38573d086b5ec158f72f11283a0",
  "pass.json": "ef3f648e787a16ac49fff2c0daa8615e1fa15df9"
}
```
- Write the manifest object to a new file called `manifest.json` in the top-level directory of the source for the pass.
- **Signature creation:**  
    Produce a detached **PKCS #7 signature** of the manifest using the private key of your Pass Type certificate.  
    Save the resulting signature file as `signature` in the same directory.
- **Bundle packaging:**  
    Include all necessary files — `pass.json`, images, `manifest.json`, and `signature`.  
    Compress the directory into a ZIP archive and rename the extension to `.pkpass`.  
    Avoid including metadata or hidden files (e.g., `.DS_Store`).
    
This `.pkpass` file is the final, distributable pass that users can add to Wallet.
## Validation and Testing
Each generated pass should be verified before distribution.  
A valid `.pkpass` can be tested by dropping it onto an iPhone or running it in the iOS Simulator — the Wallet app should display the “Add Pass” dialog if everything is correct.
## Common Issues
When a pass fails to build or install, check that:
- `pass.json` contains all **required keys**.
- `passTypeIdentifier` and `teamIdentifier` match the signing certificate.
- The **certificate** is valid and present on the signing machine.
- `manifest.json` lists **every file**, including those in subdirectories.
- All **images exist** and are in correct formats.
- `pass.json` and `manifest.json` use valid JSON syntax.
- ISO 8601 is used for **date values**.
- Localization folders (`.lproj`) follow correct naming conventions and contain complete sets of images and `pass.strings` files.
- Localized string keys match between `pass.json` and all `pass.strings`.
# Distributing and Updating a Pass
Once a pass has been built and signed, it must be distributed in a way that allows users to easily install and keep it synchronized across their devices.  
In a membership system—where each user owns a personal, static pass containing their unique QR code and identifying data—the distribution step ensures that every `.pkpass` file can be downloaded, stored, and reinstalled as needed.
## Distribution Methods
Apple Wallet supports **three official distribution channels**:
1. **From an app or App Clip**
    - Integrate a `PKAddPassButton` ([doc](https://developer.apple.com/documentation/PassKit/PKAddPassButton)) in the interface to indicate availability.
    - When tapped, display a `PKAddPassesViewController` ([doc](https://developer.apple.com/documentation/PassKit/PKAddPassesViewController)) containing the pass.
2. **From a web page**
    - Offer a downloadable link or button labeled _“Add to Apple Wallet.”_
    - Clicking the button triggers the download of the `.pkpass` file.
    - The button must follow Apple’s official [Add to Apple Wallet Guidelines](https://developer.apple.com/documentation/walletpasses/adding-a-web-service-to-update-passes).
3. **By email**
    - Send the `.pkpass` file as an attachment.
    - Users can open the attachment directly in Apple Wallet.

For automated systems, web-based or email distribution is typically preferred, as each pass file can be dynamically generated and served to the user upon request.
## Updating Passes
To modify an existing pass, issue a **new version** using the same `passTypeIdentifier` and `serialNumber`.  
When the user installs this new version, it automatically **replaces** the older one on all devices linked to their Apple ID.

Optionally, a **web service** can be implemented to deliver live updates (e.g., changing event data or dynamic fields). However, for static membership passes where information does not change, reissuing a new `.pkpass` file with the same identifiers is sufficient.
## Bundling Multiple Passes
If users need to download several passes at once (for example, in a multi-account or family plan scenario), you can create a **bundle**:
- Compress multiple `.pkpass` files into a `.zip`.
- Rename the archive’s extension to `.pkpasses`.
- Distribute the `.pkpasses` bundle using any of the same methods as a single pass.

**Bundle limitations:**
- Maximum of **10 passes** per bundle.
- Total bundle size must not exceed **150 MB**.
- MIME type for distribution: `application/vnd.apple.pkpasses`.