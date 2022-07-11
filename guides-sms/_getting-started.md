# Getting Started

You want to start using Mailjet's SMS service and are not sure where to begin? We've got your back - please follow these essential steps to get started.

## Add Money to Mailjet's SMS Wallet

Having credits in your SMS Wallet is a requirement for sending SMS. If you do not yet have funds in your wallet, you can deposit directly in [the Mailjet app](https://app.mailjet.com/sms/subscription).

Accepted currencies are USD, EUR and GBP and the minimum deposit amount is 1 unit in the respective currency.

Keep in mind that due to necessary verification procedures there is a 48-hour waiting period before your wallet is activated. See [Payments & Billing](#payment-and-billing) for additional information.

## Retrieve Your Access Token

When using the Mailjet SMS API, the authentication of your requests is done using Bearer Tokens. Bearer Tokens are a compact, URL-safe means of representing claims to be transferred between two parties.

To generate a new token, please go Mailjet's [SMS Dashboard](https://app.mailjet.com/sms) and click on _'Generate a token'_.

Keep in mind that for security reasons a token is only displayed once. Make sure to save the token value locally before closing the modal window.

Any additional information can be found in our [detailed guide](https://app.mailjet.com/docs/transactional-sms#sms-token).

## Send Your First SMS

Your balance has just been credited and your account activated - now let's send your first SMS.

The endpoint to be called is [https://api.mailjet.com/v4/sms-send](/sms-api/v4/sms-send/).

You must be authenticated with a Bearer Token. That's a different method from the one used for Emails - your access token must be included in the **Authorization header** of your request.

> cURL Example:

```bash
curl -X POST \
  https://api.mailjet.com/v4/sms-send \
  -H "Authorization: Bearer $MJ_TOKEN" \
  -H 'content-type: application/json' \
  -d '{
  "From": "InfoSMS",
  "To": "+33600000000",
  "Text": "Hello World!"
}'
```
```javascript
const mailjet = require('node-mailjet')
  .smsConnect(process.env.MJ_API_TOKEN)

const request = mailjet
  .post('sms-send', { version: 'v4' })
  .request({
    From: "InfoSMS",
    To: "+33600000000",
    Text: "Hello World!"
  })

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```

Fill your sending information in the payload. Three properties are required:

- `From` : the sender of the SMS
- `To` : the telephone number of the recipient
- `Text` : the content of the message

<div></div>
