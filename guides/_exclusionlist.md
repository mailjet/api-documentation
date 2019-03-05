# Exclusion List

The exclusion list offers the possibility to block a contact from receiving any marketing emails independently from the list it belongs too.

The contacts will still receive all transactional emails, including ones with a campaign tag.

It is possible to manage contacts individually or in bulk.

## Contact exclusion

```php
<?php
/*
Call to add contact to exclusion list
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'IsExcludedFromCampaigns' => "true"
];
$response = $mj->put(Resources::$Contact, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Call to add contact to exclusion list
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$ID_OR_EMAIL \
	-H 'Content-Type: application/json' \
	-d '{
		"IsExcludedFromCampaigns":"true"
	}'
```
```javascript
/**
 *
 * Call to add contact to exclusion list
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.put("contact")
	.id($ID_OR_EMAIL)
	.request({
		"IsExcludedFromCampaigns":"true"
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```ruby
# Call to add contact to exclusion list
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
target = Mailjet::Contact.find($ID_OR_EMAIL)
target.update_attributes(is_excluded_from_campaigns: "true"
)
p variable.attributes['Data']
```
```python
"""
Call to add contact to exclusion list
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_OR_EMAIL'
data = {
  'IsExcludedFromCampaigns': 'true'
}
result = mailjet.contact.update(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Call to add contact to exclusion list
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
	mr := &Request{
	  Resource: "contact",
	  ID: RESOURCE_ID,
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contact {
      IsExcludedFromCampaigns: "true",
    },
	}
	err := mailjetClient.Put(fmr)
	if err != nil {
	  fmt.Println(err)
	}
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
     * Call to add contact to exclusion list
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contact.resource, ID)
			.property(Contact.ISEXCLUDEDFROMCAMPAIGNS, "true");
      response = client.put(request);
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
      /// Call to add contact to exclusion list
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
            .Property(Contact.IsExcludedFromCampaigns, "true");
         MailjetResponse response = await client.PutAsync(request);
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "2014-06-10T08:30:32Z",
			"DeliveredCount": "",
			"Email": "Mister@mailjet.com",
			"ExclusionFromCampaignsUpdatedAt": "2014-06-10T08:47:28Z",
			"ID": "1",
			"IsExcludedFromCampaigns": "true",
			"IsOptInPending": "false",
			"IsSpamComplaining": "false",
			"LastActivityAt": "2014-06-10T08:47:28Z",
			"LastUpdateAt": "2014-12-31T11:00:46Z",
			"Name": "John",
			"UnsubscribedAt": "",
			"UnsubscribedBy": ""
		}
	],
	"Total": 1
}
```


You can add a contact in the exclusion list with a <code>PUT</code> with the property <code>IsExcludedFromCampaigns</code> at <code>true</code> on the [\contact](/reference/email/contacts/contact/#v3_put_contact_contact_ID) resource.

To remove a contact from the exclusion list, <code>IsExcludedFromCampaigns</code> needs to be set at <code>false</code> with a similar API call.

<div></div>

## Bulk exclusion

<div id="managing-multiple-contacts-with-contact-managemanycontacts"></div>

### Manage multiple contacts with /contact/managemanycontacts

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Contacts' => [
        [
            'Email' => "jimsmith@example.com",
            'Name' => "Jim",
            'IsExcludedFromCampaigns' => true,
            'Properties' => [
                'Property1' => "value",
                'Property2' => "value2"
            ]
        ],
        [
            'Email' => "janetdoe@example.com",
            'Name' => "Janet",
            'IsExcludedFromCampaigns' => true,
            'Properties' => [
                'Property1' => "value",
                'Property2' => "value2"
            ]
        ]
    ]
];
$response = $mj->post(Resources::$ContactManagemanycontacts, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : Manage the details of a Contact.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/managemanycontacts \
	-H 'Content-Type: application/json' \
	-d '{
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"IsExcludedFromCampaigns": true,
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
						"IsExcludedFromCampaigns": true,
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				}
		]
	}'
```
```javascript
/**
 *
 * Create : Manage the details of a Contact.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contact")
	.action("managemanycontacts")
	.request({
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"IsExcludedFromCampaigns": true,
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
						"IsExcludedFromCampaigns": true,
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				}
		]
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```ruby
# Create : Manage the details of a Contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact_managemanycontacts.create(contacts: [{ 'Email'=> 'jimsmith@example.com', 'Name'=> 'Jim', 'IsExcludedFromCampaigns'=> true, 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}, { 'Email'=> 'janetdoe@example.com', 'Name'=> 'Janet', 'IsExcludedFromCampaigns'=> true, 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}])
```
```python
"""
Create : Manage the details of a Contact.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Contacts': [
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"IsExcludedFromCampaigns": "true",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
						"IsExcludedFromCampaigns": "true",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				}
		]
}
result = mailjet.contact_managemanycontacts.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage the details of a Contact.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	var data []resources.ContactManagemanycontacts
	mr := &Request{
	  Resource: "contact",
	  Action: "managemanycontacts",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.ContactManagemanycontacts {
      Contacts: []MailjetContact {
        MailjetContact {
          Email: "jimsmith@example.com",
          Name: "Jim",
          IsExcludedFromCampaigns: true,
          Properties: MyPropertiesStruct {
            Property1: "value",
            Property2: "value2",
          },
        },
        MailjetContact {
          Email: "janetdoe@example.com",
          Name: "Janet",
          IsExcludedFromCampaigns: true,
          Properties: MyPropertiesStruct {
            Property1: "value",
            Property2: "value2",
          },
        },
      },
    },
	}
	err := mailjetClient.Post(fmr, &data)
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
import com.mailjet.client.resource.ContactManagemanycontacts;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage the details of a Contact.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactManagemanycontacts.resource)
						.property(ContactManagemanycontacts.CONTACTS, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Email", "jimsmith@example.com")
                                                                    .put("Name", "Jim")
                                                                    .put("IsExcludedFromCampaigns", "true")
                                                                    .put("Properties", new JSONObject()
                                                                        .put("Property1", "value")
                                                                        .put("Property2", "value2")))
                                                                .put(new JSONObject()
                                                                    .put("Email", "janetdoe@example.com")
                                                                    .put("Name", "Janet")
                                                                    .put("IsExcludedFromCampaigns", "true")
                                                                    .put("Properties", new JSONObject()
                                                                        .put("Property1", "value")
                                                                        .put("Property2", "value2"))));
      response = client.post(request);
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
	/// Create : Manage the details of a Contact.
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
                Resource = ContactManagemanycontacts.Resource,
            }
		.Property(ContactManagemanycontacts.Contacts, new JArray {
                new JObject {
                 {"Email", "jimsmith@example.com"},
                 {"Name", "Jim"},
                 {"IsExcludedFromCampaigns", "true"},
                 {"Properties", new JObject {
                  {"Property1", "value"},
                  {"Property2", "value2"}
                  }}
                 },
                new JObject {
                 {"Email", "janetdoe@example.com"},
                 {"Name", "Janet"},
                 {"IsExcludedFromCampaigns", "true"},
                 {"Properties", new JObject {
                  {"Property1", "value"},
                  {"Property2", "value2"}
                  }}
                 }
                });
            MailjetResponse response = await client.PostAsync(request);
            if (response.IsSuccessStatusCode)
            {
                Console.WriteLine(string.Format("Total: {0}, Count: {1}\n", response.GetTotal(), response.GetCount()));
                Console.WriteLine(response.GetData());
            }
            else
            {
                Console.WriteLine(string.Format("StatusCode: {0}\n", response.StatusCode));
                Console.WriteLine(string.Format("ErrorInfo: {0}\n", response.GetErrorInfo()));
                Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
            }
	}
    }
}
```


The same way, you can upload contacts asynchronously in one or more list with <code>/contact/managemanycontacts</code>. You can add/remove contacts from exclusion list by specifying the value of the <code>IsExcludedFromCampaigns</code> contact property.

<div id="managing-contacts-exclusion-through-csv-upload"></div>

### Manage Contacts exclusion through CSV upload

```php
<?php
/*
Create: A wrapper for the CSV importer
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'DataID' => "$ID_DATA",
    'Method' => "excludemarketing"
];
$response = $mj->post(Resources::$Csvimport, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create: A wrapper for the CSV importer
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/csvimport \
	-H 'Content-Type: application/json' \
	-d '{
		"DataID":"$ID_DATA",
		"Method":"excludemarketing"
	}'
```
```javascript
/**
 *
 * Create: A wrapper for the CSV importer
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("csvimport")
	.request({
		"DataID":"$ID_DATA",
		"Method":"excludemarketing"
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```ruby
# Create: A wrapper for the CSV importer
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Csvimport.create(data_id: "$ID_DATA",
method: "excludemarketing"
)
p variable.attributes['Data']
```
```python
"""
Create: A wrapper for the CSV importer
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'DataID': '$ID_DATA',
  'Method': 'excludemarketing'
}
result = mailjet.csvimport.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create: A wrapper for the CSV importer
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
	var data []resources.Csvimport
	mr := &Request{
	  Resource: "csvimport",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Csvimport {
      DataID: "$ID_DATA",
      Method: "excludemarketing",
    },
	}
	err := mailjetClient.Post(fmr, &data)
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
import com.mailjet.client.resource.Csvimport;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create: A wrapper for the CSV importer
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Csvimport.resource)
			.property(Csvimport.DATAID, "$ID_DATA")
			.property(Csvimport.METHOD, "excludemarketing");
      response = client.post(request);
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
      /// Create: A wrapper for the CSV importer
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
            Resource = Csvimport.Resource,
         }
            .Property(Csvimport.DataID, "$ID_DATA")
            .Property(Csvimport.Method, "excludemarketing");
         MailjetResponse response = await client.PostAsync(request);
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


Using the [Contacts CSV Upload](#csv_upload_contacts) methodology, you can upload a list of contacts to add or remove from the exclusion list with the <code>excludemarketing</code> and <code>includemarketing</code>. After uploading your CSV file, you just need to <code>POST</code> call the <code>/csvimport</code> resource with the exclusion list Methods. You can then monitor the process with the <code>JobID</code>.

The JSON payload sent to `/csvimport` should not contain the property `ContactsListID`. You will receive the following error if you include a `ContactsListID`: "MJ08 Property ContactsList is invalid: Exclusion of contacts from marketing campaign is a global status and can not be applied to a list"

<div></div>
