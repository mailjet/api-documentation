# SMS Statistics

## Retrieve List of Messages

```shell
# View : Retrieve a list of SMS messages sent during a specific time period
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms?FromTS=1033552800&ToTS=1033574400
```

With `GET` on the [`/sms`](/sms-api/v4/sms/) endpoint you are able to view details for a list of existing messages - `Status`, `Cost`, `MessageID`, sender and recipient, `CreationTS` and `SentTS` timestamps etc.

A maximum of 21 items can be retrieved per call, but a default pagination is provided in order to ensure that you have access to the full data you need.

<div></div>

>API Response

```json
{
  "Data":[
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"744ecf8c-9fed-4ec9-acd0-9b326671f5df",
      "CreationTS": 1033552800,
      "SentTS": 1033552802,
      "SMSCount":1,
      "Cost": {
        "Value": 0.04,
        "Currency": "EUR"
      }
    },
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"fabed50c-8c84-4472-bd48-8cccc13ef829",
      "CreationTS": 1033552810,
      "SentTS": 1033552812,
      "SMSCount":2,
      "Cost": {
        "Value": 0.08,
        "Currency": "EUR"
      }
    }
  ],
}
```

Use `FromTS` and `ToTS` to specify a timeframe - if no timeframe is selected, Mailjet will automatically pull the stats for the last 3 months. Please note that messages will be ordered based on the `CreationTS` property, from the most recently created to the oldest one.

<div></div>

## Filter by Recipient

```shell
# View : Retrieve a list of SMS messages sent to a specific recipient
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms?To=+33600000000
```

>API Response

```json
{
  "Data":[
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"744ecf8c-9fed-4ec9-acd0-9b326671f5df",
      "CreationTS": 1033552800,
      "SentTS": 1033552802,
      "SMSCount":1,
      "Cost": {
        "Value": 0.04,
        "Currency": "EUR"
      }
    },
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"d78b4d97-a001-4924-8184-804013f1b11a",
      "CreationTS": 1033552821,
      "SentTS": 1033552825,
      "SMSCount":3,
      "Cost": {
        "Value": 0.12,
        "Currency": "EUR"
      }
    }
  ],
}
```

If you want to view the stats for a specific recipient, simply use the `To` filter with your request.

Keep in mind that the complete phone number should be entered, as described in the [Send SMS API section](#send-transactional-sms).

<div></div>

## Stats by Message Delivery Status

```shell
# View : Retrieve a list of SMS messages that were successfully sent, or are being currently sent
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms?StatusCode=1,2
```

>API Response

```json
{
  "Data":[
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"744ecf8c-9fed-4ec9-acd0-9b326671f5df",
      "CreationTS": 1033552821,
      "SentTS": 1033552802,
      "SMSCount":1,
      "Cost": {
        "Value": 0.04,
        "Currency": "EUR"
      }
    },
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 1,
        "Name": "sent_pending",
        "Description": "Message is being sent"
      },
      "MessageId":"fabed50c-8c84-4472-bd48-8cccc13ef829",
      "CreationTS": 1033552832,
      "SentTS": "",
      "SMSCount":2,
      "Cost": {
        "Value": 0.08,
        "Currency": "EUR"
      }
    }
  ],
}
```

Use the `StatusCode` filter with your request to see only messages with specific statuses - e.g. Rejected ones because of an invalid phone number.

For more information on Status Codes and what they correspond to, please see the [list of status codes](#list-of-status-codes).

<div></div>

### List of Status Codes

| Code | Name                         | Description                                                                                  |
|------|------------------------------|----------------------------------------------------------------------------------------------|
| 0    | unknown                      | There is no record in Mailjet for the status code returned by the provider.                  |
| 1    | sent_pending                 | Message is being sent                                                                        |
| 2    | sent                         | Message sent                                                                                 |
| 3    | delivered                    | Message delivered                                                                            |
| 4    | rejected_operator            | Message rejected by the operator                                                             |
| 5    | rejected_undelivered         | Message declared as "undelivered" by the operator                                            |
| 6    | rejected_expired             | Message has been sent to the operator but expired                                            |
| 7    | rejected_incorrect_delivery  | Message has been sent to the operator but the delivery report is not correct                 |
| 8    | rejected_network             | Message has been sent to the operator but encountered a network or setup issue               |
| 9    | rejected_dnd                 | Message has been sent to the operator but recipient is subscribed to Do Not Disturb services |
| 10   | rejected_from_blacklist      | Message rejected because sender is blacklisted                                               |
| 11   | rejected_to_blacklist        | Message rejected because recipient is blacklisted                                            |
| 12   | rejected_anti_flood          | Message has been rejected due to an anti-flooding mechanism                                  |
| 13   | rejected_system_error        | Message rejected due to an expected system error                                             |
| 14   | invalid_phone_number         | The phone number specified is invalid.                                                       |
| 15   | rejected_insufficent_funds   | Message is rejected as you have not enough funds to send the message.                        |
| 16   | rejected_daily_limit_reached | Message is rejected as you have reached your maximum number of messages sent for today.      |

## Retrieve Single SMS message

```shell
# View : Retrieve a specific SMS Message
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms/744ecf8c-9fed-4ec9-acd0-9b326671f5df
```

>API Response

```json
{
  "Data":[
    {
      "From": "MJPilot",
      "To":"+33600000000",
      "Status": {
        "Code": 2,
        "Name": "sent",
        "Description": "Message sent"
      },
      "MessageId":"744ecf8c-9fed-4ec9-acd0-9b326671f5df",
      "CreationTS": 1033552800,
      "SentTS": 1033552802,
      "SMSCount":1,
      "Cost": {
        "Value": 0.04,
        "Currency": "EUR"
      }
    }
  ]
}
```

Use `GET /sms/{id}`, if you need to retrieve a specific SMS message. Substitute `{id}` with the `MessageID` of the SMS you are interested in.

<div></div>

## Retrieve SMS Count

```shell
# View : Retrieve the number of SMS messages that were processed within a specific timeframe
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms/count?FromTS=1033552800&ToTS=1033574400
```

>API Response

```json
{
  "Count": 31
}
```

Use `GET` on [`/sms/count`](/sms-api/v4/sms-count/) to retrieve the number of messages already processed. You can filter the results by entering a period of time (with `FromTS` and `ToTS`) or a status you are interested in (with `StatusCode`).

<div></div>
