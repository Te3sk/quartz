TODO ADD METADATA

## 1. Create a Google Wallet API Issuer account
A Google Wallet API Issuer account lets you create passes and issue them to Google Wallet users. An Issuer Account also gives you access to the Google Wallet API Dashboard in the [Google Pay & Wallet console](https://goo.gle/wallet-console), where you can manage your account.

To sign up for a Google Wallet API Issuer account, do the following:

1. Go to the [Google Pay & Wallet console](https://goo.gle/wallet-console) and sign in with a Google Account you want to have the 'Admin' role for your Issuer account.
2. Complete the form to provide the public business name for your Issuer account, and agree the Google Wallet API Additional Terms of Service and the Google privacy policy.
3. On the Google Pay & Wallet Console dashboard, click the 'Create a pass' button in the 'Google Wallet API' card.
4. Click the 'Build your first pass' button.
5. Review and agree to the Google Wallet API Terms of Service to create your Issuer account.

After your Google Wallet API Issuer account is created, you'll be taken to the Google Wallet API Dashboard.
## 2. Generate authentication credentials
To use the Google Wallet REST API and Android SDK, you must generate and register credentials that will be used to authenticate that your client has permission to make requests on behalf of a specific issuer account. These credentials will be sent with every request to the Google Wallet API, and ensure you Issuer account remains secure.

<div style="max-width: 800px; display: grid; grid-template-columns: auto auto; grid-template-rows: 1fr; gap: 20px; margin: 0 auto; align-items: center;">
	<a href="https://developers.google.com/wallet/tickets/events/getting-started/auth/rest?authuser=1" style="background-color: #ada27c; color: white; padding: 10px 20px; border-radius: 6px; text-decoration: none; display: inline-block; text-align: center">
		Generate REST API Credential
	</a>
	<a href="https://developers.google.com/wallet/tickets/events/getting-started/auth/android?authuser=1" style="background-color: #ada27c; color: white; padding: 10px 20px; border-radius: 6px; text-decoration: none; display: inline-block; text-align: center">
		Generate Android SDK Credential
	</a>
</div>

### Generate REST API Credential
#### 1. Enable the Google Wallet REST API
To enable the Google Wallet REST API, do the following:

1. If you don't already have a Google Cloud account, go to the [Google Cloud console](https://console.cloud.google.com/?authuser=1) and follow the steps to register for a new account.
2. From the project drop-down menu at the top of the console, select the Google Cloud project you want to use, or create a new one.
3. Go to the [Google Wallet API product details page](https://console.cloud.google.com/apis/library/walletobjects.googleapis.com?authuser=1) in the Google Cloud console Marketplace.
4. Click the 'Enable' button. In a few moments, the Google Wallet REST API will be available for the selected Google Cloud project.
#### 2. Generate a Google Cloud service account key
To authenticate requests to the Google Wallet REST API, you'll need to create a service account, and generate a service account key by doing the following:

1. Go to the ['Create service account' page](https://console.cloud.google.com/iam-admin/serviceaccounts/create?authuser=1) in the Google Cloud console.
2. Fill in the service account details. Note the email address for the service account that appears below the 'Service account ID' field. You will need this later when you use the service account to authenticate your requests to the Google Wallet REST API.
3. Click the 'DONE' button. There is no need to complete the other service account creation steps.
4. Click the 'KEYS' menu item at the top of the page.
5. Click the 'ADD KEY' drop-down menu, then click 'Create new key'.
6. Select key type 'JSON'.
7. Click 'CREATE' to create and download the service account key.
#### 3. Authorize your service account in the Google Wallet console
To authenticate your requests to the Google Wallet REST API using a service account key, you must add the email address of your service account as a user to your Issuer account. To add your service account as a user, do the following:

1. Go to the ['Service accounts'](https://console.cloud.google.com/iam-admin/serviceaccounts?authuser=1) page in the Google Cloud console.
2. Copy the email address of the service account you want to use to authenticate your requests to the Google Wallet API.
3. Go to the [Google Pay & Wallet console](https://pay.google.com/business/console/?authuser=1).
4. In the left nav, click 'Users'.
5. Click 'Invite a user'.
6. Input the email address of your service account.
7. In the 'Access level' drop-down, select 'Developer'.
8. Click the 'Invite' button.

Once your service account is added, you can use any service account keys generated for it to authenticate requests to the Google Wallet REST API. When using service account keys, keep in mind that these are highly sensitive credentials that should only be used in secure, server-side environments.

## 3. Create your first Passes Class
A Passes Class can be thought of as a shared template that passes are created from. A Passes Class defines certain properties that will be included in all passes that use it. A Pass Issuer can create multiple classes, each with their own distinctive set of properties that define attributes like style and appearance, as well as additional features like Smart Tap, and the Enrollment and Sign In.

Before applying for publishing access, you must create at least one Passes Class. The easiest way to create your first Passes Class is in the Google Wallet Business Console.

<a href="https://developers.google.com/wallet/tickets/events/use-cases/create?authuser=1" style="background-color: #ada27c; color: white; padding: 10px 20px; border-radius: 6px; text-decoration: none; display: inline-block;">
  Create a Pass Class
</a>
## 4. Build and test your first pass!
Once you have completed all of the onboarding steps in this guide, you are ready to start issuing passes to your users with the Google Wallet API. To get started, check out our tutorials and other resources to help you learn to build your first pass.

<a href="https://developers.google.com/wallet/tickets/events/getting-started/build-your-first-pass?authuser=1" style="background-color: #ada27c; color: white; padding: 10px 20px; border-radius: 6px; text-decoration: none; display: inline-block;">
Build your first pass
</a>


---
# Generic Pass
The Generic Pass is available for when your use case doesn't fit into any of the other predefined pass types. Unlike other passes that include fields and features that are specific to a use case, such as tickets, loyalty cards, and offers, the Generic Pass is intended to be flexible enough to support a variety of purposes by providing fields where you can define custom labels and values.
## Requirements
To issue passes with the Google Wallet API, you will first need to do the following:
- Create a [[#Create Google Wallet API Issuer Account|Google Wallet API Issuer account]].
- Non-Android developers: Create a [Google Cloud account](https://console.cloud.google.com/freetrial).
- Android developers: [Set up Google Play services.](https://developers.google.com/android/guides/setup)
### Create Google Wallet API Issuer Account
[Official Docs](https://developers.google.com/wallet/generic/getting-started/issuer-onboarding
A Pass Issuer is any person or company that uses the Google Wallet API to issue any type of pass to Google Wallet users. Before you can create and issue passes, you must sign up for a Google Wallet API Issuer account. Pass Issuers who want to use the Google Wallet REST API must additionally enable the Google Wallet API in the Google Cloud console.
To sign up for a Google Wallet API Issuer account, do the following:

1. Go to the [Google Pay & Wallet console](https://goo.gle/wallet-console) and sign in with a Google Account you want to have the 'Admin' role for your Issuer account.
2. Complete the form to provide the public business name for your Issuer account, and agree the Google Wallet API Additional Terms of Service and the Google privacy policy.
3. On the Google Pay & Wallet Console dashboard, click the 'Create a pass' button in the 'Google Wallet API' card.
4. Click the 'Build your first pass' button.
5. Review and agree to the Google Wallet API Terms of Service to create your Issuer account.

After your Google Wallet API Issuer account is created, you'll be taken to the Google Wallet API Dashboard.
## Get Authentication Credentials
### 1. Enable the Google Wallet REST API
To enable the Google Wallet REST API, do the following:
1. If you don't already have a Google Cloud account, go to the [Google Cloud console](https://console.cloud.google.com/) and follow the steps to register for a new account.
2. From the project drop-down menu at the top of the console, select the Google Cloud project you want to use, or create a new one.
3. Go to the [Google Wallet API product details page](https://console.cloud.google.com/apis/library/walletobjects.googleapis.com) in the Google Cloud console Marketplace.
4. Click the 'Enable' button. In a few moments, the Google Wallet REST API will be available for the selected Google Cloud project.
### 2. Generate a Google Cloud service account key
To authenticate requests to the Google Wallet REST API, you'll need to create a service account, and generate a service account key by doing the following:

1. Go to the ['Create service account' page](https://console.cloud.google.com/iam-admin/serviceaccounts/create) in the Google Cloud console.
2. Fill in the service account details. Note the email address for the service account that appears below the 'Service account ID' field. You will need this later when you use the service account to authenticate your requests to the Google Wallet REST API.
3. Click the 'DONE' button. There is no need to complete the other service account creation steps.
4. Click the 'KEYS' menu item at the top of the page.
5. Click the 'ADD KEY' drop-down menu, then click 'Create new key'.
6. Select key type 'JSON'.
7. Click 'CREATE' to create and download the service account key.
### 3. Authorize your service account in the Google Wallet console
To authenticate your requests to the Google Wallet REST API using a service account key, you must add the email address of your service account as a user to your Issuer account. To add your service account as a user, do the following:

1. Go to the ['Service accounts'](https://console.cloud.google.com/iam-admin/serviceaccounts) page in the Google Cloud console.
2. Copy the email address of the service account you want to use to authenticate your requests to the Google Wallet API.
3. Go to the [Google Pay & Wallet console](https://pay.google.com/business/console/).
4. In the left nav, click 'Users'.
5. Click 'Invite a user'.
6. Input the email address of your service account.
7. In the 'Access level' drop-down, select 'Developer'.
8. Click the 'Invite' button.

Once your service account is added, you can use any service account keys generated for it to authenticate requests to the Google Wallet REST API. When using service account keys, keep in mind that these are highly sensitive credentials that should only be used in secure, server-side environments.
## Request Authentication
First, perform the necessary library imports and define some variables for the service account JSON, and IDs for the issuer, class, unique user and object that will be saved.
Next, use one of the framework libraries to retrieve the necessary credentials to call the Google Wallet API.
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
	'/content/key.json')
	# Set up authenticated client
	self.auth()

def auth(self):
	"""Create authenticated HTTP client using a service account file."""
	self.credentials = Credentials.from_service_account_file(
	self.key_file_path,
	scopes=['https://www.googleapis.com/auth/wallet_object.issuer'])
	self.client = build('walletobjects', 'v1', credentials=self.credentials)
```
## Create Passes Classes and Passes Objects
A Passes Class can be thought of as a shared template that passes are created from. A Passes Class defines certain properties that will be included in all passes that use it. A Pass Issuer can create multiple classes, each with their own distinctive set of properties that define attributes like style and appearance, as well as additional features like Smart Tap, and the Enrollment and Sign In.
Passes Classes may be created using the Google Wallet REST API, Google Wallet Android SDK, or in the Google Wallet Business Console.
For new users, the Business Console is the easiest way to get started with creating a Passes Class, as it provides a simple user interface, where you can define the various fields of your first Passes Class by filling out form fields.
For advanced users, creating Passes Classes programmatically is the best approach.
### Use the Google Wallet Business Console
To create a Passes Class in the Google Wallet Business console, do the following:

1. Go to the [Google Pay and Wallet Business Console](https://pay.google.com/business/console/) and sign in with your Google Wallet API Issuer Account.
2. On the "Google Wallet API" card, click the "Manage passes" button.
3. Under 'Get publishing access', click the 'Create a class' button.
4. Choose a pass type from the dialog. Google Wallet offers various pass types (Event Ticket, Offer, Loyalty Card, etc.). For a flexible use case, select "Generic" as your pass type.
5. Fill in the appropriate values for the required fields.
6. Click the 'Create class' button to save your class.
### Use the Google Wallet REST API

To create a Passes Class using the Google Wallet REST API, send a `POST` request to `https://walletobjects.googleapis.com/walletobjects/v1/genericClass`. For more information, see the [reference documentation](https://developers.google.com/wallet/reference/rest/v1/genericclass/insert).
```python
def create_class(self, issuer_id: str, class_suffix: str) -> str:
    """Create a class.
    Args:
        issuer_id (str): The issuer ID being used for this request.
        class_suffix (str): Developer-defined unique ID for this pass class.

    Returns:
        The pass class ID: f"{issuer_id}.{class_suffix}"
    """

    # Check if the class exists
    try:
        self.client.genericclass().get(resourceId=f'{issuer_id}.{class_suffix}').execute()
    except HttpError as e:
        if e.status_code != 404:
            # Something else went wrong...
            print(e.error_details)
            return f'{issuer_id}.{class_suffix}'
    else:
        print(f'Class {issuer_id}.{class_suffix} already exists!')
        return f'{issuer_id}.{class_suffix}'

    # See link below for more information on required properties
    # https://developers.google.com/wallet/generic/rest/v1/genericclass
    new_class = {'id': f'{issuer_id}.{class_suffix}'}

    response = self.client.genericclass().insert(body=new_class).execute()

    print('Class insert response')
    print(response)

    return f'{issuer_id}.{class_suffix}'
```
## Create a Passes Object
A Passes Object is an instance of a Passes Class. In order to create a Passes Object, you must provide the following attributes:

- `classId`: The `id` of the Passes Class
- `id`: A unique ID for your customer

Code sample to create a Passes Object:
```python
def create_object(self, issuer_id: str, class_suffix: str,
                  object_suffix: str) -> str:
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
        if e.status_code != 404:
            # Something else went wrong...
            print(e.error_details)
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
                'uri':
                    'https://farm4.staticflickr.com/3723/11177041115_6e6a3b6f49_o.jpg'
            },
            'contentDescription': {
                'defaultValue': {
                    'language': 'en-US',
                    'value': 'Hero image description'
                }
            }
        },
        'textModulesData': [{
            'header': 'Text module header',
            'body': 'Text module body',
            'id': 'TEXT_MODULE_ID'
        }],
        'linksModuleData': {
            'uris': [{
                'uri': 'http://maps.google.com/',
                'description': 'Link module URI description',
                'id': 'LINK_MODULE_URI_ID'
            }, {
                'uri': 'tel:6505555555',
                'description': 'Link module tel description',
                'id': 'LINK_MODULE_TEL_ID'
            }]
        },
        'imageModulesData': [{
            'mainImage': {
                'sourceUri': {
                    'uri':
                        'http://farm4.staticflickr.com/3738/12440799783_3dc3c20606_b.jpg'
                },
                'contentDescription': {
                    'defaultValue': {
                        'language': 'en-US',
                        'value': 'Image module description'
                    }
                }
            },
            'id': 'IMAGE_MODULE_ID'
        }],
        'barcode': {
            'type': 'QR_CODE',
            'value': 'QR code'
        },
        'cardTitle': {
            'defaultValue': {
                'language': 'en-US',
                'value': 'Generic card title'
            }
        },
        'header': {
            'defaultValue': {
                'language': 'en-US',
                'value': 'Generic header'
            }
        },
        'hexBackgroundColor': '#4285f4',
        'logo': {
            'sourceUri': {
                'uri':
                    'https://storage.googleapis.com/wallet-lab-tools-codelab-artifacts-public/pass_google_logo.jpg'
            },
            'contentDescription': {
                'defaultValue': {
                    'language': 'en-US',
                    'value': 'Generic card logo'
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
Once complete, your customer's Passes Object will have been created on the server. However, at this stage, the Passes Object has not been linked to a Google user or their device. For the pass to be associated with a Google Wallet user, you must issue the pass.
