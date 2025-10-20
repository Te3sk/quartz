---
title: Google Wallet Workflow
date: 2025-10-19
tags:
  - api
  - tools
  - ui
category: Wallet API
status: completed
author: Te3sk
description: A Python-based workflow that authenticates with Google Wallet API, creates and manages Generic Passes, and generates secure “Save to Google Wallet” links via signed JWTs.
---
This notebook demonstrates a complete **Google Wallet API workflow** for creating and managing **Generic Passes** via the [[Google Wallet API Key Concept & Terminology#Google Wallet REST API|REST API]].  
It includes project configuration, authenticated API client setup, class and object creation, and the generation of a **Save to Google Wallet** link using a signed JWT.  
The workflow leverages the official Google client libraries to handle authentication, pass creation, and JWT signing, ensuring secure and automated integration with the Google Wallet ecosystem.
# Requirements
## Required Libraries
Install the following dependencies using `pip`:
- `google-api-python-client` ([doc](https://googleapis.github.io/google-api-python-client/docs/)) → provides the interface to interact with the **Google Wallet REST API**.
```bash
pip install google-api-python-client
```
- `google-auth` ([doc](https://google-auth.readthedocs.io/en/latest/)) → handles **service account authentication** and secure credential management.
```bash
pip install google-auth
```
- `google-auth-httplib2` ([doc](https://googleapis.dev/python/google-auth-httplib2/latest/)) and `google-auth-oauthlib` ([doc](https://google-auth-oauthlib.readthedocs.io/en/latest/)) → enable authenticated HTTP communication between your Python client and Google’s API endpoints.
```bash
pip install google-auth-httplib2 google-auth-oauthlib
```
## Environment Variables
Before running the notebook, set up the following environment variable:
`export GOOGLE_APPLICATION_CREDENTIALS="/path/to/key.json"`
This variable must point to the **service account key JSON file** that grants access to the Google Wallet Issuer account.
## Google Cloud Setup
[Official Doc - Create and delete service account keys](https://cloud.google.com/iam/docs/keys-create-delete)
- Enable the **Google Wallet API** in your Google Cloud project.
- Create or use an existing **service account** with the role:
    - `Wallet Objects Editor` (minimum)
    - or `Wallet Objects Admin` for full control.
- Invite the service account’s email to your **Issuer account** in the Google Pay & Wallet Console.
- Note your **Issuer ID**, which uniquely identifies your Wallet issuer account.
# Workflow
The following code implements the complete **Google Wallet integration workflow** — from authentication to pass creation and link generation.  
It defines a reusable `DemoGeneric` class to manage API interactions, handle credentials via a service account, and perform key operations such as creating a Generic Object and generating a **“Save to Google Wallet”** URL.  
Each function encapsulates a specific step of the process, ensuring modularity, reusability, and secure communication with Google’s Wallet API.
## Project Variables
```python
ISSUER_ID = "1289000000013217205"
CLASS_SUFFIX = "unique-name-for-the-class"
CLASS_ID = f"{ISSUER_ID}.{CLASS_SUFFIX}" # 1289000000013217205.unique-name-for-the-class
```
These variables define the **core identifiers** required to interact with the Google Wallet API and uniquely reference your pass classes and objects.  
- **`ISSUER_ID`** → The unique numeric identifier of your **Google Wallet Issuer account**, assigned by Google. It represents the entity authorized to create and manage Wallet passes within your organization.  
- **`CLASS_SUFFIX`** → A developer-defined string that acts as a readable and unique identifier for a specific Wallet class (e.g., `"membership-card"` or `"event-pass"`). Each Issuer can have multiple classes, each distinguished by its suffix.  
- **`CLASS_ID`** → The **fully qualified class identifier**, created by concatenating `ISSUER_ID` and `CLASS_SUFFIX` in the format `{issuer_id}.{class_suffix}`. This value must be used in all API requests referencing a Wallet class or any of its related objects.  
Together, these variables ensure consistent identification of your Wallet resources across API calls and projects.
## Request Authentication
```python
import json
import os
import uuid

from googleapiclient.discovery import build
from googleapiclient.errors import HttpError
from google.oauth2.service_account import Credentials
from google.auth import jwt, crypt

class DemoGeneric:
	"""Demo class for creating and managing Generic passes in Google Wallet.
	
	Attributes:
	key_file_path: Path to service account key file from Google Cloud
	Console. Environment variable: GOOGLE_APPLICATION_CREDENTIALS.
	base_url: Base URL for Google Wallet API requests.
	"""
	
	def __init__(self):
		self.key_file_path = os.environ.get('GOOGLE_APPLICATION_CREDENTIALS',
		'/path/for/key.json')
		
		# Set up authenticated client
		self.auth()
	
	def auth(self):
		"""Create authenticated HTTP client using a service account file."""
		self.credentials = Credentials.from_service_account_file(
			self.key_file_path,
			scopes=['https://www.googleapis.com/auth/wallet_object.issuer'])
			
		self.client = build('walletobjects', 'v1', credentials=self.credentials)
```
This section establishes a secure and authenticated connection with the **Google Wallet API** using a **service account**.  
The `DemoGeneric` class encapsulates the setup logic, loading credentials from the environment variable `GOOGLE_APPLICATION_CREDENTIALS` and building an authenticated API client through the `googleapiclient.discovery` interface.  
The `auth()` method initializes the client with the required scope —  
`https://www.googleapis.com/auth/wallet_object.issuer` — which grants permission to create, update, and manage Wallet objects.  
Once instantiated, this class provides a reusable foundation for interacting with Wallet API resources, ensuring that all requests are **authenticated, scoped correctly, and securely signed** with your service account credentials.
## Create a Passes Object
```python
def create_object(self, issuer_id: str, class_suffix: str, object_suffix: str) -> str:
	"""Create an object.
	
	Args:
	issuer_id (str): The issuer ID being used for this request.
	class_suffix (str): Developer-defined unique ID for the pass class.
	object_suffix (str): Developer-defined unique ID for the pass object.
	  
	Returns:
	The pass object ID: f"{issuer_id}.{object_suffix}"
	"""
	# Check if the object exists
	try:
		self.client.genericobject().get(resourceId=f'{issuer_id}.{object_suffix}').execute()
	except HttpError as e:
		# FIX: con googleapiclient lo status è in e.resp.status
		if e.resp.status != 404:
			# Something else went wrong...
			print("GET object error:", e)
			return f'{issuer_id}.{object_suffix}'
		else:
			print(f'Object {issuer_id}.{object_suffix} already exists!')
			return f'{issuer_id}.{object_suffix}'
	
	# See link below for more information on required properties
	# https://developers.google.com/wallet/generic/rest/v1/genericobject
	new_object = {
		'id': f'{issuer_id}.{object_suffix}',
		'classId': f'{issuer_id}.{class_suffix}',
		'state': 'ACTIVE',
		'heroImage': {
			'sourceUri': {
				'uri': 'https://www.svgrepo.com/show/508699/landscape-placeholder.svg'
			},
			'contentDescription': {
				'defaultValue': {
					'language': 'en-US',
					'value': 'description of the hero image'
				}
			}
		},
		'barcode': {
			'type': 'QR_CODE',
			# Il valore è una stringa pura (anche URL); il QR viene generato da Google
			'value': 'https://www.youtube.com/watch?v=xvFZjo5PgG0',
		},
		'cardTitle': {
			'defaultValue': {
				'language': 'en-US',
				'value': 'Title of the Card'
			}
		},
		"subheader": {
			"defaultValue": {
				"language": "en-US",
				"value": "Placeholder"
			}
		},
		'header': {
			'defaultValue': {
				'language': 'en-US',
				'value': 'Placeholder'
			}
		},
		'hexBackgroundColor': '#ff3131',
		'logo': {
			'sourceUri': {
				'uri':
				'https://www.svgrepo.com/show/508699/landscape-placeholder.svg'
			},
			'contentDescription': {
				'defaultValue': {
					'language': 'en-US',
					'value': 'Description of the logo'
				}
			}
		}
	} 
	
	# Create the object
	response = self.client.genericobject().insert(body=new_object).execute()
	
	print('Object insert response')
	print(response)
	
	return f'{issuer_id}.{object_suffix}'
```
The `create_object()` method is responsible for **creating or reusing a Generic Object** in Google Wallet.  
It first checks whether an object with the given identifier (`issuer_id.object_suffix`) already exists by performing a `GET` request via the `genericobject()` endpoint. If the object exists, the function returns its ID without duplication; if it does not, a new object is created and linked to the corresponding Wallet class (`issuer_id.class_suffix`).  
The function builds a **structured payload** containing all visual and functional elements of the pass — including images, barcodes, text modules, color theme, and logo — and then sends it to the Wallet API through an `insert()` request.  
The method returns the full **object ID** (`"{issuer_id}.{object_suffix}"`) and logs the API’s response, providing a clear, repeatable process for dynamically generating and managing Wallet passes via Python.
## Create Saver Link
The `create_jwt()` method generates a secure **“Save to Google Wallet”** link for an existing Generic Object.  
It uses the **service account key** specified by `key_path` to load credentials, extract the service account email, and sign a **JSON Web Token (JWT)** that authorizes Wallet to reference a specific object (`issuer_id.object_suffix`).  

The function builds a payload with `typ: "savetowallet"` and includes the `genericObjects` array pointing to the object’s ID. The JWT is then cryptographically signed with the **RSA private key** of the service account to ensure authenticity.  

Finally, the function constructs and returns the complete **Save to Wallet URL** (`https://pay.google.com/gp/v/save/<JWT>`), which users can open to instantly add the pass to their Google Wallet.  
This step enables seamless integration between your backend system and the user’s Wallet app without requiring manual actions or public endpoints.

```python
from google.oauth2 import service_account
from google.auth import crypt
from google.auth import jwt as ga_jwt

def create_jwt(self, key_path, issuer_id, object_suffix) -> str:
    """Creates and returns a **Save to Google Wallet** URL for adding an **existing**
  Generic Object to a user's Wallet, identified by `issuer_id.object_suffix`.

  Args:
    key_path (str): Path to the JSON key file of the **service account**
      with permissions for the Issuer (e.g., "/content/key.json").
    issuer_id (str): Numeric ID of the Google Wallet Issuer (e.g., "3388...").
    object_suffix (str): Unique suffix of the object (e.g., "user_001");
      concatenated as `f"{issuer_id}.{object_suffix}"`.

  Returns:
    str: A complete URL in the format `https://pay.google.com/gp/v/save/<JWT>` ready for sharing.
        The JWT is signed using the service account key and contains the payload
        `genericObjects: [{ "id": "<issuer_id>.<object_suffix>" }]`.

  Notes:
    - This function **does not create** the object; it assumes the object already exists in Wallet.
    - The JWT is signed using the service account's RSA private key (`google-auth`).
    - Ensure the service account has been invited to the Issuer account and
      that the Google Wallet API is enabled in your Google Cloud project.

  Example:
    >>> url = create_jwt(self, "/content/key.json", "3388000000023017107", "user_001")
    >>> print(url)  # Open this link on a mobile device to add the pass to Google Wallet
  """

  # Load the service account credentials from the specified file path.
  creds = service_account.Credentials.from_service_account_file(key_path)
  # Extract the service account email from the credentials.
  sa_email = creds.service_account_email

  # format the object id
  object_id = issuer_id + '.' + object_suffix

  # JWT "Save to Wallet" – variante che riferisce un oggetto già esistente
  claims = {
      "iss": sa_email, # The issuer of the JWT (service account email).
      "aud": "google", # The audience of the JWT (always "google").
      "typ": "savetowallet", # The type of the JWT (indicates a Save to Wallet request).
      "payload": {
          "genericObjects": [
              {"id": object_id} # The ID of the existing pass object to be saved.
          ]
      }
  }

  # Create an RSA signer from the service account key file.
  signer = crypt.RSASigner.from_service_account_file(key_path)
  # Encode the claims into a signed JWT using the RSA signer and decode it to a UTF-8 string.
  signed_jwt = ga_jwt.encode(signer, claims).decode("utf-8")

  # Construct the "Save to Wallet" URL using the signed JWT.
  save_url = f"https://pay.google.com/gp/v/save/{signed_jwt}"
  # Return the generated Save to Wallet URL.
  return save_url
```
## Execution
```python
# Instantiate the DemoGeneric class
demo = DemoGeneric()

# Show info about the class: path of the key file, credential type, service account email, avaiable scopes, client object, methods avaiable on client object
print("\nKey file path:", demo.key_file_path)
print("\nCredentials type:", type(demo.credentials))
print("\nService account email:", demo.credentials.service_account_email)
print("\nScopes:", demo.credentials.scopes)
print("\nClient type:", type(demo.client))
print("\n", dir(demo.client))
print("\n#-#-#-#-#-#-#-#-#-#-#-#\n")

# Check Issuer Access
demo.client.issuer().get(resourceId=ISSUER_ID).execute()

# Check the class existence
demo.client.genericclass().get(resourceId=(ISSUER_ID + "." + CLASS_SUFFIX)).execute()

print("\n#-#-#-#-#-#-#-#-#-#-#-#\n")

# You can add code here to call other methods of the DemoGeneric class
# to demonstrate the main feature, for example:
# demo.create_generic_class()
# demo.create_generic_object()
# demo.add_message_to_generic_object()
# demo.get_generic_object()

object_suffix = "uniqe-name-for-the-suffix"

create_object(demo, ISSUER_ID, CLASS_SUFFIX, object_suffix)

# GENERATE "save to Google Wallet" LINK AND PRINT IT
url = create_jwt(demo, demo.key_file_path, ISSUER_ID, object_suffix)
print("msg:", url)
```
This final section executes the complete **Google Wallet API workflow**.
It first instantiates the `DemoGeneric` class, prints diagnostic information about the environment (such as the key path, service account email, authorized scopes, and client object), and verifies that access to the **Issuer** is properly configured and that the associated **Generic Class** exists.

Once verification is complete, the workflow proceeds in three main steps:

1. **Create or reuse a pass object:**  
   The function `create_object(demo, ISSUER_ID, CLASS_SUFFIX, object_suffix)` creates (or reuses, if already existing) a Generic Object tied to the specified class, using a unique `object_suffix`.
2. **Generate the “Save to Wallet” link:**  
   The function `create_jwt(demo, demo.key_file_path, ISSUER_ID, object_suffix)` creates a signed JWT token with the service account key and builds the final URL that allows the user to add the pass directly to their Google Wallet.

In summary, this execution phase performs the full lifecycle:  
**verification → object creation → link generation**, providing a self-contained and automated workflow for testing and deploying Wallet passes.

