# Send SMS API

## Send Transactional SMS

Mailjet allows you to send transactional SMS messages using our Send SMS API.

The following API endpoint has been designed to provide this service: POST [`/sms-send`](/sms-api/v4/sms-send/).

The input payload needs to include the following array of properties:

- `From`: an alphanumeric sender ID, which can be customized for your convenience. Keep in mind that depending on the local mobile carrier policy it may be converted to a short code. See ‘[Country Specific Limitations](#country-specific-limitations)’ for more information.
- `To`: the phone number of the recipient. It needs to comply with the E.164 international telephone numbering standard. The phone number should start with a +, followed by the country code, followed by the phone number itself.
- `Text`: The content of the SMS message. See [Concatenation & Encoding](#concatenation-and-encoding) for additional information on the allowed length and message encoding.

## Authentication

In order to invoke a service protected by an access token, it is necessary to **include the access token in the Authorization header**. The Security proxy will then validate it.

```shell
# Create : Send SMS Message to a selected recipient.
curl -X POST \
 https://api.mailjet.com/v4/sms-send \
  -H "Authorization: Bearer $MJ_TOKEN" \
  -H 'Content-Type: application/json' \
  -d '{
    "Text": "Have a nice SMS flight with Mailjet !",
    "To": "+33600000000",
    "From": "MJPilot"
  }'
```

In the code sample you can see a `POST` request with an Authorization header that includes the token value.

<div></div>

>API Response

```json
{
  "From": "MJPilot",
  "To": "+33600000000",
  "Text": "Have a nice SMS flight with Mailjet !",
  "MessageId": "2034075536371630429",
  "SmsCount": 1,
  "CreationTS": 1521626400,
  "SentTS": 1521626402,
  "Cost": {
    "Value": 0.0012,
    "Currency": "EUR"
  },
  "Status": {
    "Code": 2,
    "Name": "sent",
    "Description": "Message sent"
  }
}
```

The Send SMS API will send a response, which will include the `Status` of the message, as well as its `MessageID`, time of creation, `SmsCount` (how many SMS parts were needed to send the whole message) and `Cost`.

<div></div>
