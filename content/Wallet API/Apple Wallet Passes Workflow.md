---
title: Apple Wallet Passes Workflow
date: 2025-10-21
tags:
  - api
  - wallet
  - back-end
  - ui
  - workflow
category: Wallet API
status: in_corso
author: Te3sk
description: TODO
---
# 1. Create the Source for a Pass
[[Apple Wallet Passes#Creating the Source for a Pass|What it is and what it is used for]]
1. Create the source folder 
	- Make a directory named as the final pass name, suffixed with `.pass` (e.g., `Member123.pass`).
	- This folder is the top-level source for the pass.
2. Add the core JSON
	- Inside the folder, create `pass.json`.
	- Put all display strings and pass metadata in [this file](https://developer.apple.com/documentation/walletpasses/pass). Below an example containing **only required fields**:
```json
{
	"description": "This is a trial pass for the Apple Wallet",
	"formatVersion": 1,
	"organizationName": "Easy Go Visit",
	"passTypeIdentifier": "pass.com.example.test",
	"serialNumber": "1234567890",
	"teamIdentifier": "XXXXXXXXXX"
}
```
1. Add required images
	- Include the pass **icon** (`icon.png`; add `icon@2x.png` / `icon@3x.png` if you provide multiple resolutions).
	- This icon is used in notifications, Lock Screen, and email attachments.
2. Add other visual assets (if you use them)
	- Place any additional images the pass type supports (e.g., logo, background, thumbnail) in the same folder.
	- Provide the same image in multiple resolutions if you use them (e.g., `logo.png`, `logo@2x.png`, `logo@3x.png`).
3. (Optional) Prepare localization folders
	- For each language/region you want to support, create a folder at the top level named:  
	    `[language-identifier]-[region-identifier].lproj` (examples: `fr.lproj`, `zh-Hans.lproj`).
4. Localize images (optional)
	- Put localized images into each `.lproj` folder (e.g., `logo.png`, `logo@2x.png`, `logo@3x.png`).
	- **Each locale folder must contain the same number of resolutions for a given image.**
	- Note: a same-named image placed at the **top level** overrides localized versions; also, a localized image **may not update** after the pass is already added to Wallet.
5. Use automatic localization where applicable
	- For fields that are dates/times/currencies in `pass.json`, supply values in standard formats (e.g., ISO 8601 for dates).
	- The system will localize those automatically based on the device settings.
```json
{
  "dateStyle": "PKDateStyleShort",
  "isRelative": true,
  "key": "expires",
  "label": "ExpiresLabel",
  "value": "2019-06-26T12:00:00+00:00"
}
```
8. Set up keys for custom (manual) string localization (optional)
	- In `pass.json`, use **keys** (not literals) for any strings you want to translate:
```json
"primaryFields": [
  { "key": "offer", "value": "OfferAmount", "label": "OfferAmountLabel" }
]
```
9. Provide translated [[Apple Wallet Passes#Localizing Text Fields|strings per locale]] (optional)
	- In each `.lproj` folder, add `pass.strings` and map the keys used in `pass.json` to localized text:
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
	- Use **UTF-16** encoding for non-ASCII characters in `.strings` files.
# 2. Building a Pass
[[Apple Wallet Passes#Building a Pass|What it is and what it is used for]]
1. Prepare the source
	- Create the **source files** for the pass (see [[#1. Create the Source for a Pass|Create the Source for a Pass]]).
	- This source will later be turned into the distributable bundle.
2. Create a [[Apple Wallet Passes#Pass Type Identifier|Pass Type Identifier]]
	- In your [Apple Developer account](https://developer.apple.com/account) → **Certificates, Identifiers & Profiles**:
	    - Go to **Identifiers** → **Add (+)** → choose **Pass Type IDs** → **Continue**.
	    - Enter a description and a **reverse-DNS string** to create the identifier (e.g., `com.example-company.passes.membership`).
	- In your `pass.json`:
	    - Set `"passTypeIdentifier"` to that identifier.
	    - Set `"serialNumber"` to a **unique** value (each identifier + serialNumber pair defines one unique pass; issuing the same pair overwrites the existing pass on a device).
3. Generate a [[Apple Wallet Passes#Create a Signing Certificate|signing certificate]]
	- [Create a certificate signing request (CSR)](https://developer.apple.com/help/account/create-certificates/create-a-certificate-signing-request).
	- In [Apple Developer account](https://developer.apple.com/account) → **Certificates, Identifiers & Profiles**:
	    - Go to **Certificates** → **Add (+)**.
	    - Choose **Pass Type ID Certificate** → **Continue**.
	    - Name the certificate, select your Pass Type ID, and **upload the CSR**.
	    - **Generate** and **download** the certificate to the machine that will sign the pass.
4. [[Apple Wallet Passes#Signing and Packaging the Pass|Sign the pass and create the bundle]]
	- In the **top level** of the pass source:
	    1. Generate a **manifest** of the source files.
	    2. Write it to **`manifest.json`** (keys = relative file paths; values = **SHA1** hashes of each file, including files in subdirectories).
	    3. Create a **PKCS #7 detached signature** of `manifest.json` using the **private key of the Pass Type ID signing certificate**.
		    1. Get the **private key** (export it in `.p12` format in the Keychain Access when you get the CSR), it should be `[name].p12`
		    2. Extract the private key:
			```bash
			openssl pkcs12 -in "path/to/your/private-key.p12" -out "path/to/your/out/privatekey.pem" -nocerts -nodes
			```
			3. Extract the certificate:
			```bash
			openssl x509 -in path/to/your/pass-certificate.cer -out path/to/your/out/pass-certificate.pem -outform PEM
			```
			4. Combine private key and certificate in a `.pem` file:
			```bash
			cat path/to/your/privatekey.pem path/to/your/cert.pem > path/to/your/out/combined.pem
			```
			5. Use it to sign `manifest.json`:
			```bash
			openssl smime -sign -in path/to/your/manifest.json -out path/to/your/out/signature -signer path/to/your/combined.pem -inkey path/to/your/privatekey.pem -certfile path/to/your/pass-certificate.cer
			```
	    4. Save that signature as a file named **`signature`** at the top level of the pass.
	    5. **Zip** the directory.
	    6. Rename the archive from `.zip` to **`.pkpass`**.
	- **Do not include** metadata files not part of the format (e.g., `.DS_Store`) in the manifest or distributable pass.
6. Test
	- Drop the `.pkpass` onto an **iPhone running in Simulator**; Wallet shows the **Add Pass** dialog if the pass is valid.
# Distributing and Updating a Pass
[[Apple Wallet Passes#Distributing and Updating a Pass|What it is and what it is used for]]
1. Choose a [[Apple Wallet Passes#Distribution Methods|distribution channel]] (pick one or more)
	- **App/App Clip**: add a `PKAddPassButton`; on tap, present a `PKAddPassesViewController` with the pass.
	- **Web page**: show an **Add to Apple Wallet** button and download the `.pkpass` when clicked (follow the _Add to Apple Wallet Guidelines_).
	- **Email**: send the `.pkpass` as an email attachment.
2. Prepare updates (if you need to refresh a user’s pass)
	- Distribute a **new version of the pass** using the **same** `passTypeIdentifier` **and** `serialNumber` to overwrite the existing pass on the user’s device(s).
	- (Optional per the doc) You **may** provide a web service to update pass contents.
3. (Optional) Offer multiple passes at once
	- Create a **bundle of passes** by:
	    - Zipping the individual `.pkpass` files.
	    - Renaming the archive from `.zip` to **`.pkpasses`**.
	- Distribute this bundle the **same ways** as a single pass.
	- Respect the limits: **max 10 passes** or **150 MB** total size.
	- Use the MIME type: **`application/vnd.apple.pkpasses`**.

---
