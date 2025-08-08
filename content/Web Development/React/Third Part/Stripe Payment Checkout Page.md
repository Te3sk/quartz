---
title: Stripe Payment
date: 2025-07-31
tags:
  - api
  - front-end
  - react
  - ui
category: React Development
status: in_corso
author: Te3sk
description: Guide to embed Strype payment form
---
# Stripe Payment Form
There are 3 way to embed a Stripe payment form in a react web app:
1. **Create a Stripe-hosted checkout page:** customers enter their payment details in a Stripe-hosted payment page, then return to your site after payment completion. ![Stripe-hosted page example|450](https://b.stripecdn.com/docs-statics-srv/assets/checkout-hosted-hover.180c6ab2498a8c65daefb5bedae835bf.png)
2. **Embed a payment form on the site:** customers enter their payment details in an embedded form on your site without redirection. ![Embedded form example|450](https://b.stripecdn.com/docs-statics-srv/assets/checkout-embedded-hover.14466c835d9723cfe90b3549956c451a.png)
3. **Build a checkout page with embedded components:** customers enter their payment details in your customized checkout page on your site. ![checkout with embedded components example|450](https://b.stripecdn.com/docs-statics-srv/assets/checkout-elements-hover.bfd33fb56dc4ec8915e4ab4601799f49.png)

# TODO 
# TODO
# Build a checkout Page whit embedded components
First install the Stripe library:
``` bash
npm install stripe@18.0.0 --save
```
## Configuration
There must be a configuration file in `[project]/src/config/` containing the initialization of the stripe library:
```js
// stripe import
import { loadStripe } from "@stripe/stripe-js";

const stripe_publishable_key = ...;

/**
* Initialize the Stripe library
*
* @returns {Promise<Stripe>} - The Stripe instance
*/
export async function initializeStripe() {

const stripe = await loadStripe(stripe_publishable_key);

return stripe;
}
```
The stripe publishable key are used **to identify your account with Stripe**, is a good practice to **import it from `.env` file**.
In this file you can put all the other configuration functions, if needed.
## Business Logic
There must be an other file in `[project]/src/lib/` containing all the utils functions to handle the payment. The most important are:
```js
// firebase auth import
import { getIdToken } from 'firebase/auth';
  
/**
* Create a payment intent. The payment intent is created on the server side and then sent to the client side.
*
* It rapresents the payment method and the amount of the payment.
*
* @param {Object} auth - The Firebase auth instance
* @param {string} plan - The plan to create the payment intent for
* @param {string} promoCode - The promo code to apply to the payment intent
* @returns {Promise<Object>} - The payment intent in the form of { clientSecret, discountInfo, status }
* @throws {Object} - The error in the form of { message, code, status }
*/
export const createPaymentIntent = async (auth, plan, promoCode = '') => {
	try {
		// Get the user's token
		const token = await getIdToken(auth.currentUser, true);
		
		// Create the payment intent
		const response = await fetch(`${import.meta.env.VITE_API_URL}/create-payment-intent`, {
			method: 'POST',
			headers: {
				Authorization: `Bearer ${token}`,
				'Content-Type': 'application/json',
			},
			body: JSON.stringify({ plan, promoCode }),
		});
		
		// Get the data from the response
		const data = await response.json();
		
		// If there is an error, throw it
		if (data.error) {
			throw {
				message: data.error || "An error occurred while creating the payment intent",
				code: "PAYMENT_INTENT/GENERIC_ERROR",
				status: 500,
			};
		} 
		
		return {
			clientSecret: data.clientSecret,
			discountInfo: data.discountInfo,
			status: 200,
		};
	} catch (error) {
		throw {
			message: error.message || "An error occurred while creating the payment intent",
			code: "PAYMENT_INTENT/GENERIC_ERROR",
			status: 500,
		};
	}
};

/**
* Validate the billing form.
*
* If the form is not valid, the errors are returned.
*
* If the form is valid, the function returns an empty array.
*
* @param {Object} formData - The form data
* @returns {Array} - The errors
*/
export const validateBillingForm = (formData) => {
// Initialize the errors array
	const errors = [];
	
	// Define the required fields
	const requiredFields = [
		'nomeCognomeRagioneSociale',
		'indirizzo',
		'citta',
		'provincia',
		'cap',
		'codiceFiscalePartitaIva'
	];
	  
	// Check if the required fields are present
	requiredFields.forEach(field => {
		if (!formData[field] || formData[field].trim() === '') {
			errors.push(field);
		}
	}); 
	
	// Check if the terms are accepted
	if (!formData.isTermsChecked) {
		errors.push('isTermsChecked');
	}
	
	// Return the errors
	return errors;
};

/**
* Confirm the payment using the Stripe API.
*
* @param {Object} stripe - The Stripe instance
* @param {Object} elements - The elements instance
* @param {string} clientSecret - The client secret
* @param {Object} billingData - The billing data
* @returns {Promise<Object>} - The result in the form of { success, error, paymentIntent, status }
* @throws {Object} - The error in the form of { message, code, status }
*/
export const confirmPayment = async (stripe, elements, clientSecret, billingData) => {
	try {
		// Confirm the payment by API call
		const result = await stripe.confirmCardPayment(clientSecret, {
			payment_method: {
				card: elements.getElement('cardNumber'),
				billing_details: {
					name: billingData.nomeCognomeRagioneSociale,
					address: {
						line1: billingData.indirizzo,
						city: billingData.citta,
						state: billingData.provincia,
						postal_code: billingData.cap,
						country: 'IT',
					},
				},
			},
		});
		
		// Return the result
		return {
			success: result.paymentIntent?.status === 'succeeded',
			error: result.error?.message || null,
			paymentIntent: result.paymentIntent,
			status: 200,
		};
	} catch (error) {
	// Return the error
		throw {
			message: error.message || "An error occurred while confirming the payment",
			code: "PAYMENT/GENERIC_ERROR",
			status: 500,
		};
	}
};
```
- **`createPaymentIntent`**:  
  Authenticates the user with Firebase, then sends a request to the backend to create a Stripe Payment Intent based on the selected plan and promo code. Returns the `clientSecret`, any discount info, and the status.
- **`validateBillingForm`**:  
  Validates the billing form fields to ensure all required fields are filled and the terms are accepted. Returns an array of missing or invalid field keys.
- **`confirmPayment`**:
  Uses the Stripe API to confirm the payment using the `clientSecret`, card details, and billing data. Returns the payment result including success status, any errors, and the paymentIntent details.
## 