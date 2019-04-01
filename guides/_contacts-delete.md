# GDPR Delete Contacts

Under the European Union's [General Data Protection Regulation (GDPR)](https://www.eugdpr.org/), recipients in your Mailjet contacts database have the right to request that you delete all their personal data stored on your end. In such cases, the GDPR requires the permanent removal of their contact record from your database, including contact properties, email tracking history and other engagement data. You’ll typically need to respond to these requests within 30 days.

With the Mailjet API you are able to comply with the GDPR policy and delete a specific contact on a specific API Key.

<aside class="notice">
To perform a GDPR-compliant deletion, you must proceed the following calls on __all accounts / subaccounts you currently own__.
</aside>

You must complete the following steps to successfully delete a contact:

1. Identify the presence of this contact in your Mailjet account.
2. Save the Mailjet `{contact_ID}` related to this recipient.
3. Proceed with the deletion using the `{contact_ID}` you retrieved.

## Retrieve a Contact



> API Response:



To delete a contact, you must first identify its presence in the contact database of your account.  

Use `GET` [`/contact/{contact_email}`](https://dev.mailjet.com/email-api/v3/contact/) to do it.

<div></div>

Save the contact ID - you need it to complete the deletion process.

## Delete the Contact

```shell
curl -s -X DELETE \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v4/contacts/{contact_ID} \
```

Use the `{contact_ID}` you retrieved to `DELETE` the contact with the `/v4/contacts/{contact_ID} endpoint`.  When the deletion is successful, the API will return a `200 OK` status. Any other response will indicate that the deletion was not successfully processed.

<div></div>

```json
{
    "Count": 1,
    "Data": [
        {
            "CreatedAt": "2017-09-26T14:12:32Z",
            "DeliveredCount": 0,
            "Email": "\\xae872d762f67cc4fc5d4bfe04e607256579928ac@domain.invalid",
            "ExclusionFromCampaignsUpdatedAt": "",
            "ID": 21339837,
            "IsExcludedFromCampaigns": false,
            "IsOptInPending": false,
            "IsSpamComplaining": false,
            "LastActivityAt": "2017-09-26T14:12:32Z",
            "LastUpdateAt": "2018-07-12T09:04:23Z",
            "Name": "Anonymized",
            "UnsubscribedAt": "",
            "UnsubscribedBy": ""
        }
    ],
    "Total": 1
}
```

<aside class="notice">
Contact details are immediately anonymized and all records will be deleted after 30 days. This process cannot be reversed. The anonymized contact will retain its contact ID and general configuration settings until it is removed when the 30-day period ends.
</aside>

<div/div>

The deletion of a contact does not prevent you from re-uploading the same contact in the future. If you are using an external database to sync contacts with your Mailjet contact database, please make sure to simultaneously remove the contacts from it as well.

This way you will be completely GDPR-compliant and will ensure that the contacts won’t be added by mistake later on.
