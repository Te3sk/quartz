---
title: Add Automatic invoice sending from Backend
date: 2025-08-05
tags:
  - front-end
  - react
  - api
category: Stripe API
status: in_corso
author: Te3sk
description: Edit backend function to make stripe send an automatic invoice to the customer
Doc Link: https://docs.stripe.com/payments/checkout/receipts?payment-ui=embedded-components#customizing-receipts-embedded-components
---
# Email receipts and paid invoices

Send receipts for payments and refunds automatically.

# Stripe-hosted page

> This is a Stripe-hosted page for when payment-ui is stripe-hosted. View the full page at https://docs.stripe.com/payments/checkout/receipts?payment-ui=stripe-hosted.

You can manually or automatically send customized email receipts or [paid invoices](https://docs.stripe.com/payments/checkout/receipts.md#paid-invoices). Learn more about [receipts for payments](https://docs.stripe.com/receipts.md).

## Automatically send receipts 

To enable automated receipts, toggle **Successful payments** on in your [Customer emails settings](https://dashboard.stripe.com/settings/emails). Receipts are only sent when a successful payment has been made—no receipt is sent if the payment fails or is declined.

## Customize receipts 

Alter the appearance and functionality of your receipts with the following customization options:

- **Branding**: Modify the logo and colors in your [Branding settings](https://dashboard.stripe.com/settings/branding). The upper limit for a custom logo image file size is 512KB. Ideally, the logo should be a square image exceeding 128 x 128 pixels. JPG, PNG, and GIF file types are supported.
- **Public information**: Specify the public information you want to include, such as your contact number or website address, in your [Public details settings](https://dashboard.stripe.com/settings/public).

To display custom text, use the [payment_intent_data.description](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-payment_intent_data-description) attribute on the [Checkout Session](https://docs.stripe.com/api/checkout/sessions/object.md). Some examples include:

- Description of goods or services provided
- Authorization code
- Subscription information
- Cancellation policies

You can see a real-time preview of your email receipt on your Dashboard Branding settings page. To send a test receipt, hover over the preview image and click **Send test receipt**, then enter your email address.

> Receipts pull data from the `Charge` object generated when the PaymentIntent is confirmed. To update receipt data such as the `description` after the charge is generated, you must [update the Charge](https://docs.stripe.com/api/charges/update.md). Changes to a confirmed PaymentIntent don’t appear on receipts.

## Automatically send paid invoices 

In addition to ordinary receipts, Checkout can generate paid invoices as proof of payment. Invoices have more information than receipts. For subscriptions, Stripe generates invoices automatically, but for one-time payments, you need to enable them.

> Invoice creation for one-time payments through the [Checkout Sessions API](https://docs.stripe.com/api/checkout/sessions.md) is not an [Invoicing](https://stripe.com/invoicing) feature, and is priced separately. Review [this support article](https://support.stripe.com/questions/pricing-for-post-payment-invoices-for-one-time-purchases-via-checkout-and-payment-links) to learn more.

To generate invoices, first, in your [Customer emails settings](https://dashboard.stripe.com/settings/emails), under **Email customers about**, select **Successful payments**. Then, when creating a Checkout session, set [invoice_creation[enabled]](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-invoice_creation-enabled) to `true`.

> Enabling `invoice_creation` isn’t supported if you set `payment_intent_data[capture_method]` to `manual`.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --data-urlencode success_url="https://example.com" \
  --data-urlencode cancel_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --success-url="https://example.com" \
  --cancel-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={"enabled": True},
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  success_url="https://example.com",
  cancel_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {"enabled": True},
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "success_url": "https://example.com",
  "cancel_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => ['enabled' => true],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'success_url' => 'https://example.com',
  'cancel_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setSuccessUrl("https://example.com")
    .setCancelUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setSuccessUrl("https://example.com")
    .setCancelUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  SuccessURL: stripe.String("https://example.com"),
  CancelURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  SuccessURL: stripe.String("https://example.com"),
  CancelURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    SuccessUrl = "https://example.com",
    CancelUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    SuccessUrl = "https://example.com",
    CancelUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

After the payment completes, Stripe sends an invoice summary with links to download the invoice PDF and invoice receipt to the email address your customer provides during checkout.

> Invoices for delayed notification payment methods might take longer to send because we send the invoice after successful payment, not upon checkout session completion. These methods include: [Bacs Direct Debit](https://docs.stripe.com/payments/bacs-debit/accept-a-payment.md), [Bank transfers](https://docs.stripe.com/payments/bank-transfers/accept-a-payment.md), [Boleto](https://docs.stripe.com/payments/boleto/accept-a-payment.md), [Canadian pre-authorized debits](https://docs.stripe.com/payments/acss-debit/accept-a-payment.md), [Konbini](https://docs.stripe.com/payments/konbini/accept-a-payment.md), [OXXO](https://docs.stripe.com/payments/oxxo/accept-a-payment.md), [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank/accept-a-payment.md), [SEPA Direct Debit](https://docs.stripe.com/payments/sepa-debit/accept-a-payment.md), [SOFORT](https://docs.stripe.com/payments/sofort/accept-a-payment.md), and [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit/accept-a-payment.md).
![Screenshot of the invoice PDF that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice.9e44668032383601eeec362f38293b7a.png)

The downloadable invoice PDF
![Screenshot of the invoice receipt that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice_receipt.4f120ee7363f8e7728fa553a8a24aae3.png)

The downloadable invoice receipt
![Screenshot of the invoice summary email Stripe sends](https://b.stripecdn.com/docs-statics-srv/assets/email.560c2666905531b907f7fcd4f1a0a6dd.png)

The customer email with links to the invoice PDF and receipt

You can also view the invoice in the [Dashboard](https://dashboard.stripe.com/invoices) or access it programmatically by listening to the [invoice.paid](https://docs.stripe.com/api/events/types.md#event_types-invoice.paid) event through an [event destination](https://docs.stripe.com/event-destinations.md).

You can use the `invoice_data` hash inside `invoice_creation` to further customize the invoice generated by the Checkout Session.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --data-urlencode success_url="https://example.com" \
  --data-urlencode cancel_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --success-url="https://example.com" \
  --cancel-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  success_url="https://example.com",
  cancel_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "success_url": "https://example.com",
  "cancel_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => [
    'enabled' => true,
    'invoice_data' => [
      'description' => 'Invoice for Product X',
      'metadata' => ['order' => 'order-xyz'],
      'account_tax_ids' => ['DE123456789'],
      'custom_fields' => [
        [
          'name' => 'Purchase Order',
          'value' => 'PO-XYZ',
        ],
      ],
      'rendering_options' => ['amount_tax_display' => 'include_inclusive_tax'],
      'footer' => 'B2B Inc.',
    ],
  ],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'success_url' => 'https://example.com',
  'cancel_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setSuccessUrl("https://example.com")
    .setCancelUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setSuccessUrl("https://example.com")
    .setCancelUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {
        order: 'order-xyz',
      },
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {
        amount_tax_display: 'include_inclusive_tax',
      },
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  success_url: 'https://example.com',
  cancel_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  SuccessURL: stripe.String("https://example.com"),
  CancelURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  SuccessURL: stripe.String("https://example.com"),
  CancelURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    SuccessUrl = "https://example.com",
    CancelUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    SuccessUrl = "https://example.com",
    CancelUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

Review [invoice best practices](https://docs.stripe.com/invoicing/customize.md) for your region to make sure you’re collecting the right information from your customers. Information like the customer’s billing and shipping addresses, phone number and tax ID appear on the resulting invoice.

## Localization 

When using Checkout Sessions, the language of the receipt and invoice is determined by several factors:

- If a Customer is set, their [preferred locale](https://docs.stripe.com/api/customers/object.md#customer_object-preferred_locales) are used if available.
- If a Customer is set without any preferred locales, the [language setting](https://dashboard.stripe.com/settings/emails) from the Stripe Dashboard is applied.
- If no Customer is set, the language defaults to the browser locale of the user opening the Checkout Session URL.


# Embedded form

> This is a Embedded form for when payment-ui is embedded-form. View the full page at https://docs.stripe.com/payments/checkout/receipts?payment-ui=embedded-form.

You can manually or automatically send customized email receipts or [paid invoices](https://docs.stripe.com/payments/checkout/receipts.md#paid-invoices-embedded-form). Learn more about [receipts for payments](https://docs.stripe.com/receipts.md).

## Automatically send receipts 

To enable automated receipts, toggle **Successful payments** on in your [Customer emails settings](https://dashboard.stripe.com/settings/emails). Receipts are only sent when a successful payment has been made—no receipt is sent if the payment fails or is declined.

## Customize receipts 

Alter the appearance and functionality of your receipts with the following customization options:

- **Branding**: Modify the logo and colors in your [Branding settings](https://dashboard.stripe.com/settings/branding). The upper limit for a custom logo image file size is 512KB. Ideally, the logo should be a square image exceeding 128 x 128 pixels. JPG, PNG, and GIF file types are supported.
- **Public information**: Specify the public information you want to include, such as your contact number or website address, in your [Public details settings](https://dashboard.stripe.com/settings/public).

To display custom text, use the [payment_intent_data.description](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-payment_intent_data-description) attribute on the [Checkout Session](https://docs.stripe.com/api/checkout/sessions/object.md). Some examples include:

- Description of goods or services provided
- Authorization code
- Subscription information
- Cancellation policies

You can see a real-time preview of your email receipt on your Dashboard Branding settings page. To send a test receipt, hover over the preview image and click **Send test receipt**, then enter your email address.

> Receipts pull data from the `Charge` object generated when the PaymentIntent is confirmed. To update receipt data such as the `description` after the charge is generated, you must [update the Charge](https://docs.stripe.com/api/charges/update.md). Changes to a confirmed PaymentIntent don’t appear on receipts.

## Automatically send paid invoices 

In addition to ordinary receipts, Checkout can generate paid invoices as proof of payment. Invoices have more information than receipts. For subscriptions, Stripe generates invoices automatically, but for one-time payments, you need to enable them.

> Invoice creation for one-time payments through the [Checkout Sessions API](https://docs.stripe.com/api/checkout/sessions.md) is not an [Invoicing](https://stripe.com/invoicing) feature, and is priced separately. Review [this support article](https://support.stripe.com/questions/pricing-for-post-payment-invoices-for-one-time-purchases-via-checkout-and-payment-links) to learn more.

To generate invoices, first, in your [Customer emails settings](https://dashboard.stripe.com/settings/emails), under **Email customers about**, select **Successful payments**. Then, when creating a Checkout session, set [invoice_creation[enabled]](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-invoice_creation-enabled) to `true`.

> Enabling `invoice_creation` isn’t supported if you set `payment_intent_data[capture_method]` to `manual`.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  -d ui_mode=embedded \
  --data-urlencode return_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --ui-mode=embedded \
  --return-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={"enabled": True},
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  ui_mode="embedded",
  return_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {"enabled": True},
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "ui_mode": "embedded",
  "return_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => ['enabled' => true],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'ui_mode' => 'embedded',
  'return_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.EMBEDDED)
    .setReturnUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.EMBEDDED)
    .setReturnUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeEmbedded),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeEmbedded),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "embedded",
    ReturnUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "embedded",
    ReturnUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

After the payment completes, Stripe sends an invoice summary with links to download the invoice PDF and invoice receipt to the email address your customer provides during checkout.

> Invoices for delayed notification payment methods might take longer to send because we send the invoice after successful payment, not upon checkout session completion. These methods include: [Bacs Direct Debit](https://docs.stripe.com/payments/bacs-debit/accept-a-payment.md), [Bank transfers](https://docs.stripe.com/payments/bank-transfers/accept-a-payment.md), [Boleto](https://docs.stripe.com/payments/boleto/accept-a-payment.md), [Canadian pre-authorized debits](https://docs.stripe.com/payments/acss-debit/accept-a-payment.md), [Konbini](https://docs.stripe.com/payments/konbini/accept-a-payment.md), [OXXO](https://docs.stripe.com/payments/oxxo/accept-a-payment.md), [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank/accept-a-payment.md), [SEPA Direct Debit](https://docs.stripe.com/payments/sepa-debit/accept-a-payment.md), [SOFORT](https://docs.stripe.com/payments/sofort/accept-a-payment.md), and [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit/accept-a-payment.md).
![Screenshot of the invoice PDF that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice.9e44668032383601eeec362f38293b7a.png)

The downloadable invoice PDF
![Screenshot of the invoice receipt that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice_receipt.4f120ee7363f8e7728fa553a8a24aae3.png)

The downloadable invoice receipt
![Screenshot of the invoice summary email Stripe sends](https://b.stripecdn.com/docs-statics-srv/assets/email.560c2666905531b907f7fcd4f1a0a6dd.png)

The customer email with links to the invoice PDF and receipt

You can also view the invoice in the [Dashboard](https://dashboard.stripe.com/invoices) or access it programmatically by listening to the [invoice.paid](https://docs.stripe.com/api/events/types.md#event_types-invoice.paid) event through an [event destination](https://docs.stripe.com/event-destinations.md).

You can use the `invoice_data` hash inside `invoice_creation` to further customize the invoice generated by the Checkout Session.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  -d ui_mode=embedded \
  --data-urlencode return_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --ui-mode=embedded \
  --return-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  ui_mode="embedded",
  return_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "ui_mode": "embedded",
  "return_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => [
    'enabled' => true,
    'invoice_data' => [
      'description' => 'Invoice for Product X',
      'metadata' => ['order' => 'order-xyz'],
      'account_tax_ids' => ['DE123456789'],
      'custom_fields' => [
        [
          'name' => 'Purchase Order',
          'value' => 'PO-XYZ',
        ],
      ],
      'rendering_options' => ['amount_tax_display' => 'include_inclusive_tax'],
      'footer' => 'B2B Inc.',
    ],
  ],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'ui_mode' => 'embedded',
  'return_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.EMBEDDED)
    .setReturnUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.EMBEDDED)
    .setReturnUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {
        order: 'order-xyz',
      },
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {
        amount_tax_display: 'include_inclusive_tax',
      },
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'embedded',
  return_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeEmbedded),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeEmbedded),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "embedded",
    ReturnUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "embedded",
    ReturnUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

Review [invoice best practices](https://docs.stripe.com/invoicing/customize.md) for your region to make sure you’re collecting the right information from your customers. Information like the customer’s billing and shipping addresses, phone number and tax ID appear on the resulting invoice.

## Localization 

When using Checkout Sessions, the language of the receipt and invoice is determined by several factors:

- If a Customer is set, their [preferred locale](https://docs.stripe.com/api/customers/object.md#customer_object-preferred_locales) are used if available.
- If a Customer is set without any preferred locales, the [language setting](https://dashboard.stripe.com/settings/emails) from the Stripe Dashboard is applied.
- If no Customer is set, the language defaults to the browser locale of the user opening the Checkout Session URL.


# Embedded components

> This is a Embedded components for when payment-ui is embedded-components. View the full page at https://docs.stripe.com/payments/checkout/receipts?payment-ui=embedded-components.

You can manually or automatically send customized email receipts or [paid invoices](https://docs.stripe.com/payments/checkout/receipts.md#paid-invoices-embedded-components). Learn more about [receipts for payments](https://docs.stripe.com/receipts.md).

## Automatically send receipts 

To enable automated receipts, toggle **Successful payments** on in your [Customer emails settings](https://dashboard.stripe.com/settings/emails). Receipts are only sent when a successful payment has been made—no receipt is sent if the payment fails or is declined.

## Customize receipts 

Alter the appearance and functionality of your receipts with the following customization options:

- **Branding**: Modify the logo and colors in your [Branding settings](https://dashboard.stripe.com/settings/branding). The upper limit for a custom logo image file size is 512KB. Ideally, the logo should be a square image exceeding 128 x 128 pixels. JPG, PNG, and GIF file types are supported.
- **Public information**: Specify the public information you want to include, such as your contact number or website address, in your [Public details settings](https://dashboard.stripe.com/settings/public).

To display custom text, use the [payment_intent_data.description](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-payment_intent_data-description) attribute on the [Checkout Session](https://docs.stripe.com/api/checkout/sessions/object.md). Some examples include:

- Description of goods or services provided
- Authorization code
- Subscription information
- Cancellation policies

You can see a real-time preview of your email receipt on your Dashboard Branding settings page. To send a test receipt, hover over the preview image and click **Send test receipt**, then enter your email address.

> Receipts pull data from the `Charge` object generated when the PaymentIntent is confirmed. To update receipt data such as the `description` after the charge is generated, you must [update the Charge](https://docs.stripe.com/api/charges/update.md). Changes to a confirmed PaymentIntent don’t appear on receipts.

## Automatically send paid invoices 

In addition to ordinary receipts, you can configure the Checkout Session to generate paid invoices as proof of payment. Invoices have more information than receipts. For subscriptions, Stripe generates invoices automatically, but for one-time payments, you need to enable them.

> Invoice creation for one-time payments through the [Checkout Sessions API](https://docs.stripe.com/api/checkout/sessions.md) is not an [Invoicing](https://stripe.com/invoicing) feature, and is priced separately. Review [this support article](https://support.stripe.com/questions/pricing-for-post-payment-invoices-for-one-time-purchases-via-checkout-and-payment-links) to learn more.

To generate invoices, first, in your [Customer emails settings](https://dashboard.stripe.com/settings/emails), under **Email customers about**, select **Successful payments**. Then, when creating a Checkout session, set [invoice_creation[enabled]](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-invoice_creation-enabled) to `true`.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  -d ui_mode=custom \
  --data-urlencode return_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --ui-mode=custom \
  --return-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {enabled: true},
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={"enabled": True},
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  ui_mode="custom",
  return_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {"enabled": True},
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "ui_mode": "custom",
  "return_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => ['enabled' => true],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'ui_mode' => 'custom',
  'return_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.CUSTOM)
    .setReturnUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder().setEnabled(true).build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.CUSTOM)
    .setReturnUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeCustom),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeCustom),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "custom",
    ReturnUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "custom",
    ReturnUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

After the payment completes, Stripe sends an invoice summary with links to download the invoice PDF and invoice receipt to the email address your customer provides during checkout.

> Invoices for delayed notification payment methods might take longer to send because we send the invoice after successful payment, not upon checkout session completion. These methods include: [Bacs Direct Debit](https://docs.stripe.com/payments/bacs-debit/accept-a-payment.md), [Bank transfers](https://docs.stripe.com/payments/bank-transfers/accept-a-payment.md), [Boleto](https://docs.stripe.com/payments/boleto/accept-a-payment.md), [Canadian pre-authorized debits](https://docs.stripe.com/payments/acss-debit/accept-a-payment.md), [Konbini](https://docs.stripe.com/payments/konbini/accept-a-payment.md), [OXXO](https://docs.stripe.com/payments/oxxo/accept-a-payment.md), [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank/accept-a-payment.md), [SEPA Direct Debit](https://docs.stripe.com/payments/sepa-debit/accept-a-payment.md), [SOFORT](https://docs.stripe.com/payments/sofort/accept-a-payment.md), and [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit/accept-a-payment.md).
![Screenshot of the invoice PDF that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice.9e44668032383601eeec362f38293b7a.png)

The downloadable invoice PDF
![Screenshot of the invoice receipt that customers can download from the invoice summary email](https://b.stripecdn.com/docs-statics-srv/assets/invoice_receipt.4f120ee7363f8e7728fa553a8a24aae3.png)

The downloadable invoice receipt
![Screenshot of the invoice summary email Stripe sends](https://b.stripecdn.com/docs-statics-srv/assets/email.560c2666905531b907f7fcd4f1a0a6dd.png)

The customer email with links to the invoice PDF and receipt

You can also view the invoice in the [Dashboard](https://dashboard.stripe.com/invoices) or access it programmatically by listening to the [invoice.paid](https://docs.stripe.com/api/events/types.md#event_types-invoice.paid) event through an [event destination](https://docs.stripe.com/event-destinations.md).

You can use the `invoice_data` hash inside `invoice_creation` to further customize the invoice generated by the Checkout Session.

```curl
curl https://api.stripe.com/v1/checkout/sessions \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  -d ui_mode=custom \
  --data-urlencode return_url="https://example.com"
```

```cli
stripe checkout sessions create  \
  --mode=payment \
  -d "invoice_creation[enabled]"=true \
  -d "invoice_creation[invoice_data][description]"="Invoice for Product X" \
  -d "invoice_creation[invoice_data][metadata][order]"=order-xyz \
  -d "invoice_creation[invoice_data][account_tax_ids][0]"=DE123456789 \
  -d "invoice_creation[invoice_data][custom_fields][0][name]"="Purchase Order" \
  -d "invoice_creation[invoice_data][custom_fields][0][value]"=PO-XYZ \
  -d "invoice_creation[invoice_data][rendering_options][amount_tax_display]"=include_inclusive_tax \
  -d "invoice_creation[invoice_data][footer]"="B2B Inc." \
  -d "line_items[0][price]"={{ONE_TIME_PRICE_ID}} \
  -d "line_items[0][quantity]"=1 \
  --ui-mode=custom \
  --return-url="https://example.com"
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
Stripe.api_key = '<<YOUR_SECRET_KEY>>'

session = Stripe::Checkout::Session.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
})
```

```ruby
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = Stripe::StripeClient.new("<<YOUR_SECRET_KEY>>")

session = client.v1.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {order: 'order-xyz'},
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {amount_tax_display: 'include_inclusive_tax'},
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
})
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
import stripe
stripe.api_key = "<<YOUR_SECRET_KEY>>"

session = stripe.checkout.Session.create(
  mode="payment",
  invoice_creation={
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  line_items=[{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  ui_mode="custom",
  return_url="https://example.com",
)
```

```python
# Set your secret key. Remember to switch to your live secret key in production.
# See your keys here: https://dashboard.stripe.com/apikeys
client = StripeClient("<<YOUR_SECRET_KEY>>")

session = client.checkout.sessions.create({
  "mode": "payment",
  "invoice_creation": {
    "enabled": True,
    "invoice_data": {
      "description": "Invoice for Product X",
      "metadata": {"order": "order-xyz"},
      "account_tax_ids": ["DE123456789"],
      "custom_fields": [{"name": "Purchase Order", "value": "PO-XYZ"}],
      "rendering_options": {"amount_tax_display": "include_inclusive_tax"},
      "footer": "B2B Inc.",
    },
  },
  "line_items": [{"price": "{{ONE_TIME_PRICE_ID}}", "quantity": 1}],
  "ui_mode": "custom",
  "return_url": "https://example.com",
})
```

```php
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
$stripe = new \Stripe\StripeClient('<<YOUR_SECRET_KEY>>');

$session = $stripe->checkout->sessions->create([
  'mode' => 'payment',
  'invoice_creation' => [
    'enabled' => true,
    'invoice_data' => [
      'description' => 'Invoice for Product X',
      'metadata' => ['order' => 'order-xyz'],
      'account_tax_ids' => ['DE123456789'],
      'custom_fields' => [
        [
          'name' => 'Purchase Order',
          'value' => 'PO-XYZ',
        ],
      ],
      'rendering_options' => ['amount_tax_display' => 'include_inclusive_tax'],
      'footer' => 'B2B Inc.',
    ],
  ],
  'line_items' => [
    [
      'price' => '{{ONE_TIME_PRICE_ID}}',
      'quantity' => 1,
    ],
  ],
  'ui_mode' => 'custom',
  'return_url' => 'https://example.com',
]);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
Stripe.apiKey = "<<YOUR_SECRET_KEY>>";

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.CUSTOM)
    .setReturnUrl("https://example.com")
    .build();

Session session = Session.create(params);
```

```java
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");

SessionCreateParams params =
  SessionCreateParams.builder()
    .setMode(SessionCreateParams.Mode.PAYMENT)
    .setInvoiceCreation(
      SessionCreateParams.InvoiceCreation.builder()
        .setEnabled(true)
        .setInvoiceData(
          SessionCreateParams.InvoiceCreation.InvoiceData.builder()
            .setDescription("Invoice for Product X")
            .putMetadata("order", "order-xyz")
            .addAccountTaxId("DE123456789")
            .addCustomField(
              SessionCreateParams.InvoiceCreation.InvoiceData.CustomField.builder()
                .setName("Purchase Order")
                .setValue("PO-XYZ")
                .build()
            )
            .setRenderingOptions(
              SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.builder()
                .setAmountTaxDisplay(
                  SessionCreateParams.InvoiceCreation.InvoiceData.RenderingOptions.AmountTaxDisplay.INCLUDE_INCLUSIVE_TAX
                )
                .build()
            )
            .setFooter("B2B Inc.")
            .build()
        )
        .build()
    )
    .addLineItem(
      SessionCreateParams.LineItem.builder()
        .setPrice("{{ONE_TIME_PRICE_ID}}")
        .setQuantity(1L)
        .build()
    )
    .setUiMode(SessionCreateParams.UiMode.CUSTOM)
    .setReturnUrl("https://example.com")
    .build();

Session session = client.checkout().sessions().create(params);
```

```node
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('<<YOUR_SECRET_KEY>>');

const session = await stripe.checkout.sessions.create({
  mode: 'payment',
  invoice_creation: {
    enabled: true,
    invoice_data: {
      description: 'Invoice for Product X',
      metadata: {
        order: 'order-xyz',
      },
      account_tax_ids: ['DE123456789'],
      custom_fields: [
        {
          name: 'Purchase Order',
          value: 'PO-XYZ',
        },
      ],
      rendering_options: {
        amount_tax_display: 'include_inclusive_tax',
      },
      footer: 'B2B Inc.',
    },
  },
  line_items: [
    {
      price: '{{ONE_TIME_PRICE_ID}}',
      quantity: 1,
    },
  ],
  ui_mode: 'custom',
  return_url: 'https://example.com',
});
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
stripe.Key = "<<YOUR_SECRET_KEY>>"

params := &stripe.CheckoutSessionParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionLineItemParams{
    &stripe.CheckoutSessionLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeCustom),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := session.New(params)
```

```go
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
params := &stripe.CheckoutSessionCreateParams{
  Mode: stripe.String(stripe.CheckoutSessionModePayment),
  InvoiceCreation: &stripe.CheckoutSessionCreateInvoiceCreationParams{
    Enabled: stripe.Bool(true),
    InvoiceData: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataParams{
      Description: stripe.String("Invoice for Product X"),
      Metadata: map[string]string{"order": "order-xyz"},
      AccountTaxIDs: []*string{stripe.String("DE123456789")},
      CustomFields: []*stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
        &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataCustomFieldParams{
          Name: stripe.String("Purchase Order"),
          Value: stripe.String("PO-XYZ"),
        },
      },
      RenderingOptions: &stripe.CheckoutSessionCreateInvoiceCreationInvoiceDataRenderingOptionsParams{
        AmountTaxDisplay: stripe.String("include_inclusive_tax"),
      },
      Footer: stripe.String("B2B Inc."),
    },
  },
  LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
    &stripe.CheckoutSessionCreateLineItemParams{
      Price: stripe.String("{{ONE_TIME_PRICE_ID}}"),
      Quantity: stripe.Int64(1),
    },
  },
  UIMode: stripe.String(stripe.CheckoutSessionUIModeCustom),
  ReturnURL: stripe.String("https://example.com"),
}
result, err := sc.V1CheckoutSessions.Create(context.TODO(), params)
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
StripeConfiguration.ApiKey = "<<YOUR_SECRET_KEY>>";

var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "custom",
    ReturnUrl = "https://example.com",
};
var service = new Stripe.Checkout.SessionService();
Stripe.Checkout.Session session = service.Create(options);
```

```dotnet
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
var options = new Stripe.Checkout.SessionCreateOptions
{
    Mode = "payment",
    InvoiceCreation = new Stripe.Checkout.SessionInvoiceCreationOptions
    {
        Enabled = true,
        InvoiceData = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataOptions
        {
            Description = "Invoice for Product X",
            Metadata = new Dictionary<string, string> { { "order", "order-xyz" } },
            AccountTaxIds = new List<string> { "DE123456789" },
            CustomFields = new List<Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions>
            {
                new Stripe.Checkout.SessionInvoiceCreationInvoiceDataCustomFieldOptions
                {
                    Name = "Purchase Order",
                    Value = "PO-XYZ",
                },
            },
            RenderingOptions = new Stripe.Checkout.SessionInvoiceCreationInvoiceDataRenderingOptionsOptions
            {
                AmountTaxDisplay = "include_inclusive_tax",
            },
            Footer = "B2B Inc.",
        },
    },
    LineItems = new List<Stripe.Checkout.SessionLineItemOptions>
    {
        new Stripe.Checkout.SessionLineItemOptions
        {
            Price = "{{ONE_TIME_PRICE_ID}}",
            Quantity = 1,
        },
    },
    UiMode = "custom",
    ReturnUrl = "https://example.com",
};
var client = new StripeClient("<<YOUR_SECRET_KEY>>");
var service = client.V1.Checkout.Sessions;
Stripe.Checkout.Session session = service.Create(options);
```

Review [invoice best practices](https://docs.stripe.com/invoicing/customize.md) for your region to make sure you’re collecting the right information from your customers. Information like the customer’s billing and shipping addresses, phone number and tax ID appear on the resulting invoice.

## Localization 

When using Checkout Sessions, the language of the receipt and invoice is determined by several factors:

- If a Customer is set, their [preferred locale](https://docs.stripe.com/api/customers/object.md#customer_object-preferred_locales) are used if available.
- If a Customer is set without any preferred locales, the [language setting](https://dashboard.stripe.com/settings/emails) from the Stripe Dashboard is applied.


## See also

- [Send customer emails](https://docs.stripe.com/invoicing/send-email.md)
- [Automate customer emails](https://docs.stripe.com/billing/revenue-recovery/customer-emails.md)