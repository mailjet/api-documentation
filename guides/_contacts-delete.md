# GDPR Delete Contacts

Under the European Union's [General Data Protection Regulation (GDPR)](https://www.eugdpr.org/), recipients in your Mailjet contacts database have the right to request the deletion of all their personal data stored on your end. In such cases, the GDPR requires the permanent removal of their contact record from your database, including contact properties, email tracking history and other engagement data. You’ll typically need to respond to these requests within 30 days.

With the Mailjet API you are able to comply with the GDPR policy and delete a specific contact on a specific API Key.

<aside class="notice">
To perform a GDPR-compliant deletion, you must remove the contacts from all accounts / subaccounts you currently own.
</aside>

You must complete the following steps to successfully delete a contact:

1. Identify the presence of this contact in your Mailjet account.
2. Save the Mailjet `{contact_ID}` related to this recipient.
3. Proceed with the deletion using the `{contact_ID}` you retrieved.

## Retrieve a Contact

```php
<?php
/*
View : Retrieve details for a contact (including contact ID) by using the contact email.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Contact, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : Retrieve details for a contact (including contact ID) by using the contact email.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$CONTACT_EMAIL 
```
```javascript
/**
 *
 * View : Retrieve details for a contact (including contact ID) by using the contact email.
 *
 */
const mailjet = require('node-mailjet')
    .apiConnect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)

const request = mailjet
  .get('contact')
  .id($CONTACT_EMAIL)
  .request()

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```
```ruby
# View : Retrieve details for a contact (including contact ID) by using the contact email.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Contact.find($CONTACT_EMAIL)
p variable.attributes['Data']
```
```python
"""
View : Retrieve details for a contact (including contact ID) by using the contact email.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$CONTACT_EMAIL'
result = mailjet.contact.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : Retrieve details for a contact (including contact ID) by using the contact email.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	var data []resources.Contact
	mr := &Request{
	  Resource: "contact",
	  ID: RESOURCE_ID,
	}
	err := mailjetClient.Get(mr, &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contact;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Retrieve details for a contact (including contact ID) by using the contact email.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contact.resource, ID);
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```csharp
using Mailjet.Client;
using Mailjet.Client.Resources;
using System;
using Newtonsoft.Json.Linq;
namespace Mailjet.ConsoleApplication
{
   class Program
   {
      /// <summary>
      /// View : Retrieve details for a contact (including contact ID) by using the contact email.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"));
         MailjetRequest request = new MailjetRequest
         {
            Resource = Contact.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
         MailjetResponse response = await client.GetAsync(request);
         if (response.IsSuccessStatusCode)
         {
            Console.WriteLine(string.Format("Total: {0}, Count: {1}\n", response.GetTotal(), response.GetCount()));
            Console.WriteLine(response.GetData());
         }
         else
         {
            Console.WriteLine(string.Format("StatusCode: {0}\n", response.StatusCode));
            Console.WriteLine(string.Format("ErrorInfo: {0}\n", response.GetErrorInfo()));
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


> API Response:

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "2018-01-01T00:01:02Z",
			"DeliveredCount": "3",
			"Email": "Mister@mailjet.com",
			"ExclusionFromCampaignsUpdatedAt": "",
			"ID": "12345678",
			"IsExcludedFromCampaigns": "false",
			"IsOptInPending": "false",
			"IsSpamComplaining": "false",
			"LastActivityAt": "2018-01-01T01:02:03Z",
			"LastUpdateAt": "",
			"Name": "MisterMailjet",
			"UnsubscribedAt": "",
			"UnsubscribedBy": ""
		}
	],
	"Total": 1
}
```


To delete a contact, you must first identify its presence in the contact database of your account.  

Use `GET` [`/contact/$CONTACT_EMAIL`](https://dev.mailjet.com/reference/email/contacts/contact/#v3_get_contact_contact_ID) to do it.

<div></div>

Save the contact ID - you need it to complete the deletion process.

## Delete the Contact

> Example available only with CURL

```shell
curl -s \
-X DELETE \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v4/contacts/{contact_ID} \
```

Use the `{contact_ID}` you retrieved to `DELETE` the contact with the `/v4/contacts/{contact_ID}` endpoint.  When the deletion is successful, the API will return a `200 OK` status. Any other response will indicate that the deletion was not successfully processed.

<div></div>

```shell
# Retrieve an anonymized contact
curl -s \
-X GET \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v3/REST/contact/{contact_ID} \
```

> API Response

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

<div></div>

The deletion of a contact does not prevent you from re-uploading the same contact in the future. If you are using an external database to sync contacts with your Mailjet contact database, please make sure to simultaneously remove the contacts from it as well.

This way you will be completely GDPR-compliant and will ensure that the contacts won’t be added by mistake later on.
