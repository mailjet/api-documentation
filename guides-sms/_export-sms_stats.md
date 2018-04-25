# Export SMS Statistics

Exporting SMS Statistics into a CSV file is an asynchronous process and consists of two separate requests:

1. Ask for your document to be generated.
2. Retrieve the link to download the CSV file, once it has been successfully created.

## CSV Generation

```shell
# Create : Create a CSV export file with a list of SMS messages for a specific time period
curl -s -X POST \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms/export \
-H 'Content-Type: application/json' \
-d '{"FromTS": 1033552800, "ToTS": 1033574400}'
```

A `POST` on the [`/sms/export`](/sms-api/v4/sms-export/) endpoint gives you the ability to request a CSV export file to be generated. With your request you are required to submit a specific timeframe using the `FromTS` and `ToTS` properties.

<aside class="notice">
Note: The timestamp used for `FromTS` can be up to one year ago.
</aside>

<div></div>

When the request is accepted a process for the creation of the document will be queued. For each state of the creation there is a different status, when the state is changed that status will be updated. When the document is created it will be uploaded to our server.

>API Response

```json
{
  "ID":56654,
  "CreationTS":null,
  "ExpirationTS":null,
  "Status":{
    "Code": 1,
    "Name": "PENDING",
    "Description": "The request is accepted."
  },
  "URL":null,
  "FromTs":1033552800,
  "ToTs":1033574400
}
```

The response will include an Request ID under the `ID` property. Make sure to save it, as you need it to check the export status and see the URL to download the CSV file.

<div></div>

## Check Status / Retrieve Export URL

```shell
# View : Check the status of an export request and retrieve the download URL, if ready
curl -s -X GET \
-H "Authorization: Bearer $MJ_TOKEN" \
https://api.mailjet.com/v4/sms/export/$ID
```

Use `GET` on [`/sms/export/{RequestID}`](/sms-api/v4/sms-export/) to check the status of your export request, and to retrieve the download URL, once the export file is ready.

<div></div>

>API Response

```json
{
  "ID":56654,
  "CreationTS":1033674500,
  "ExpirationTS":1034236100,
  "Status": {
    "Code": 3,
    "Name": "COMPLETED",
    "Description": "The export completed without errors."
  },
  "URL": "https://api.mailjet.com/v3/data/rfh9o9fn92n9u29r3hjf2.csv",
  "FromTs":1033552800,
  "ToTs":1033574400
}
```

- The `CreationTS` property will indicate the exact moment when the document is uploaded to our server.
- The `ExpirationTS` will indicate until when the document will be available for download. The expiration timestamp will be 7 days after the creation timestamp.
- The `Status` property will indicate the status of your request. Once it is complete, the `URL` property will give you the link to download the export file.
- The properties `FromTS` and `ToTS` will indicate the time window for which the data is collected.

<div></div>

### List of Export Status Codes

| Code | Name          | Description                          |
|------|---------------|--------------------------------------|
| 1    | `PENDING`     | The request is accepted.             |
| 2    | `IN_PROGRESS` | The export is in progress.           |
| 3    | `COMPLETED`   | The export completed without errors. |
| 4    | `ERROR`       | The export is in error state.        |
