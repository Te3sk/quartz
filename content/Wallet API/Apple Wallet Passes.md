metadata TODO
# Creating the Source for a Pass
Here is a **concise and complete summary** of Apple’s documentation section **“Creating the Source for a Pass”**, rewritten in English and structured clearly without losing any technical detail.
## 1. Pass Structure
Each Apple Wallet pass consists of a **source directory** containing:
- Image files (icon, logo, background, etc.)
- A **`pass.json`** file that defines:
    - The strings displayed on the pass
    - Metadata describing the pass type and fields
- Optional **localization folders** with translated strings and localized images

**Example directory structure:**
```
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
## 2. Creating the Directory
- The directory name defines the final pass name, followed by `.pass` (e.g., `Lift It Membership.pass`).
- This directory contains all required source files for building the `.pkpass` package.
- The **`pass.json`** file is mandatory and represents the core definition of the pass.

Reference: [Apple Developer – Pass Object](https://developer.apple.com/documentation/walletpasses/pass)
## 3. Adding Images
- Every pass must include at least one **icon image** (`icon.png`):
    - Displayed in notifications, on the Lock Screen, and in email attachments.
- Some pass types require additional images such as logos, backgrounds, or thumbnails.
- Always include high-resolution variants (`@2x`, `@3x`).

Reference: _Pass Design and Creation_ (Apple documentation)
## 4. Localization

To support multiple languages or regions, you can localize both text and images.
Each localization has its own directory:

```
[language]-[region].lproj
```

Examples:
- `fr.lproj` – French
- `zh-Hans.lproj` – Simplified Chinese

Each folder contains:
- Localized image assets (e.g., `logo.png`, `logo@2x.png`)
- A `pass.strings` file with translated text strings

**Important notes:**
- If an image with the same name exists in the top-level folder, it overrides localized versions.
- Once a pass is added to Wallet, localized images may not update automatically.

## 5. Localizing Strings
Apple Wallet localizes text in two ways:
### a) Automatic localization (dates, times, currencies)
- The system automatically localizes fields that use standard formats (e.g., ISO 8601 for dates).  
    Example:
```json
{
  "dateStyle": "PKDateStyleShort",
  "isRelative": true,
  "key": "expires",
  "label": "ExpiresLabel",
  "value": "2019-06-26T12:00:00+00:00"
}
```

Even if you do not provide localization folders, date and currency fields will still appear in the user’s device language.
### b) Manual localization (custom text)
For custom labels and text:
1. In `pass.json`, assign string **keys** instead of literal text:
    ```json
    "primaryFields": [
      {
        "key": "offer",
        "value": "OfferAmount",
        "label": "OfferAmountLabel"
      }
    ]
    ```
2. Create a `pass.strings` file inside each localization folder containing key-value pairs:  
    **English (`en.lproj/pass.strings`):**
    ```text
    /* English Localization */
    "OfferAmount" = "100% off";
    "OfferAmountLabel" = "Anything you want!";
    ```
    **Simplified Chinese (`zh-Hans.lproj/pass.strings`):**
    ```text
    /* Simplified Chinese Localization */
    "OfferAmount" = "100% 折扣";
    "OfferAmountLabel" = "尽享所需一切！";
    ```

**Encoding requirement:**  
Use **UTF-16** encoding for any non-ASCII characters.
# Building a Pass
## 1. Overview
A distributable pass for Apple Wallet is a **signed bundle** that contains:
- The JSON description (`pass.json`)
- Images
- Optional localization folders

**To build a pass**, you must:
1. Create the source files (see _Creating the Source for a Pass_).
2. Create a **Pass Type Identifier**.
3. Generate a **signing certificate**.
4. Create a **digital signature** for the pass.
5. Build the **signed bundle (.pkpass)**.
## 2. Create a Pass Type Identifier
A **Pass Type Identifier** groups related passes (e.g., tickets for multiple events).  
Each group has a unique identifier written in **reverse-DNS format**, such as:

```
com.example-company.passes.ticket.event-4631A
```

### Characteristics:
- Each pass in the group is uniquely identified by a **serial number**.
- The combination of **pass identifier + serial number** defines a unique pass.
- Adding a new pass with the same identifier and serial number replaces the existing one on the user’s device.
### Steps to create:
1. Log in to your **Apple Developer Account → Certificates, Identifiers & Profiles**.
2. Go to **Identifiers** and click **Add (+)**.
3. Choose **Pass Type IDs**, then click **Continue**.
4. Enter a description and a reverse-DNS string to create the identifier.
5. Set the following in your `pass.json`:
    ```json
    "passTypeIdentifier": "com.example-company.passes.ticket.event-4631A",
    "serialNumber": "00001"
    ```
6. The `passTypeIdentifier` must match the created ID.
7. The `serialNumber` must be unique for each individual pass.

Reference: _Developer Account Help – Managing Identifiers._
## 3. Generate a Signing Certificate
To sign your pass, you need a **signing certificate** linked to your Pass Type Identifier.
### Steps:
1. Generate a **Certificate Signing Request (CSR)** on your local machine.  
    (See _Create a Certificate Signing Request_ in the Apple docs.)
2. In your Apple Developer account:
    - Go to **Certificates** → click **Add (+)**.
    - Choose **Pass Type ID Certificate** → click **Continue**.
    - Enter a certificate name and select your Pass Type ID from the dropdown.
    - Upload your CSR.
3. After uploading, Apple generates the certificate.
4. **Download** the certificate to the machine that will sign your passes.

Reference: _Developer Account Help – Certificates._
## 4. Sign the Pass and Create the Bundle
To sign and bundle the pass, follow these steps:
### Step 1 – Generate the Manifest
- Create a **`manifest.json`** file at the top level of your pass directory.
- The manifest lists each source file (including files in subdirectories) and its **SHA1 hash**.
**Example:**
```json
{
  "icon.png" : "2a1625e1e1b3b38573d086b5ec158f72f11283a0",
  "pass.json" : "ef3f648e787a16ac49fff2c0daa8615e1fa15df9",
  "en.lproj/logo.png" : "cff02680b9041b7bf637960f9f2384738c935347",
  "zh-Hans.lproj/pass.strings" : "b0b4499ba7369e4cc15bad45c251e7b9bbcad6a4"
}
```

Each key is the **relative path** of the file, and each value is its **SHA1 checksum**.
### Step 2 – Create a Digital Signature
- Generate a **PKCS #7 detached signature** for the `manifest.json` file.
- Use the **private key** from the **Pass Type ID certificate**.
- Save the resulting file as **`signature`** in the top-level directory.
### Step 3 – Create the Archive
1. Zip the entire directory (without adding metadata files like `.DS_Store`).
2. Rename the archive’s extension from `.zip` to `.pkpass`.

**Do not include** system metadata files that are not part of the pass format.
### Step 4 – Test the Pass
- Drag and drop the `.pkpass` file onto an iPhone Simulator.
- Wallet will show the **“Add Pass”** dialog if the pass is valid.

## 5. Common Problems
If the pass fails to build or install, verify the following:
### File Structure and Syntax
- `pass.json` contains all required keys.
- JSON syntax is valid in both `pass.json` and `manifest.json`.
- The `manifest.json` includes every source file (including subdirectories).
- All required images are present and correctly formatted.
### Identifier and Certificate
- The `passTypeIdentifier` in `pass.json` matches the Pass Type ID of the signing certificate.
- The `teamIdentifier` in `pass.json` matches your Apple Developer Team ID.
- The machine used for signing has a copy of the certificate.
- The certificate is valid (not expired).
### Localization
- Localization folders have correct **language-region codes** (e.g., `en.lproj`, `zh-Hans.lproj`).
- Each localization folder contains:
    - All localized image files.
    - A `pass.strings` file with properly formatted key-value pairs.
- The localized string keys in `pass.json` match those in each `pass.strings`.
- Each `pass.strings` file includes the same keys and number of entries.
### Data Formats
- Values requiring structured formats are correct (e.g., `value` or `attributedValue` in `PassFieldContent` must use **ISO 8601** for dates).