# Managing Contacts in bulk

## Choosing a resource


###Managing list subscriptions for a single contact ( /contact/$ID/managecontactslists )

This resource allows to add a single contact in several lists at once.
[More information](#managecontactslists)

###Managing and uploading multiple contacts ( /contact/managemanycontacts ) 

This resource allows to add contacts in bulk in a json format. Optionally, these contacts can be added to existing lists. 
[More information](#contact_managemanycontacts)

###Managing multiple Contacts subscriptions to a list ( /contactslist/$ID/managemanycontacts ) 

This resource allows to add contacts directly to a list. This resource will create new contacts if the contacts are not already in the Mailjet system. 
[More information](#contactslist_managemanycontacts)


###Managing Contacts through CSV upload

This process allows to upload csv containing large quantities of contacts.
[More information](#csv_upload_contacts)


<h2 id="managecontactslists">Subscriptions for a contact</h2>

```php
<?php
/*
Create : Manage a contact subscription to a list
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'ContactsLists' => [
        [
            'ListID' => "$ListID_1",
            'Action' => "addnoforce"
        ],
        [
            'ListID' => "$ListID_2",
            'Action' => "addforce"
        ]
    ]
];
$response = $mj->post(Resources::$ContactManagecontactslists, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : Manage a contact subscription to a list
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$ID/managecontactslists \
	-H 'Content-Type: application/json' \
	-d '{
		"ContactsLists":[
				{
						"ListID": "$ListID_1",
						"Action": "addnoforce"
				},
				{
						"ListID": "$ListID_2",
						"Action": "addforce"
				}
		]
	}'
```
```javascript
/**
 *
 * Create : Manage a contact subscription to a list
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contact")
	.id($ID)
	.action("managecontactslists")
	.request({
		"ContactsLists":[
				{
						"ListID": "$ListID_1",
						"Action": "addnoforce"
				},
				{
						"ListID": "$ListID_2",
						"Action": "addforce"
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
# Create : Manage a contact subscription to a list
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Contact_managecontactslists.create(id: $ID, contacts_lists: [{
    'ListID'=> '$ListID_1',
    'Action'=> 'addnoforce'
}, {
    'ListID'=> '$ListID_2',
    'Action'=> 'addforce'
}]
)
p variable.attributes['Data']
```
```python
"""
Create : Manage a contact subscription to a list
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'ContactsLists': [
				{
						"ListID": "$ListID_1",
						"Action": "addnoforce"
				},
				{
						"ListID": "$ListID_2",
						"Action": "addforce"
				}
		]
}
result = mailjet.contact_managecontactslists.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage a contact subscription to a list
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
	var data []resources.ContactManagecontactslists
	mr := &Request{
	  Resource: "contact",
	  ID: RESOURCE_ID,
	  Action: "managecontactslists",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.ContactManagecontactslists {
      ContactsLists: []MailjetContactsList {
        MailjetContactsList {
          ListID: "$ListID_1",
          Action: "addnoforce",
        },
        MailjetContactsList {
          ListID: "$ListID_2",
          Action: "addforce",
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
import com.mailjet.client.resource.ContactManagecontactslists;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage a contact subscription to a list
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactManagecontactslists.resource, ID)
			.property(ContactManagecontactslists.CONTACTSLISTS, new JSONArray()
                .put(new JSONObject()
                    .put("ListID", "$ListID_1")
                    .put("Action", "addnoforce"))
                .put(new JSONObject()
                    .put("ListID", "$ListID_2")
                    .put("Action", "addforce")));
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
      /// Create : Manage a contact subscription to a list
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
            Resource = ContactManagecontactslists.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(ContactManagecontactslists.ContactsLists, new JArray {
                new JObject {
                 {"ListID", "$ListID_1"},
                 {"Action", "addnoforce"}
                 },
                new JObject {
                 {"ListID", "$ListID_2"},
                 {"Action", "addforce"}
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
			"ContactsLists" : [
				{ 
					"Action" : "addnoforce", 
					"ListID" : "$ListID_1" 
				}, 
				{ 
					"Action" : "addforce", 
					"ListID" : "$ListID_2"
				}
			] 
		}
	],
	"Total": 1
}
```


To manage a Contact subscription for one or multiple Lists, we can do a <code>POST</code> on <code>[/contact/$ID/managecontactslists](/email-api/v3/contact-managecontactslists/)</code>, specifying the contact ID in the URL and add JSON to the body of the request with the list subscriptions to be modified.


  'Action' is mandatory and can be one of the following values:

  Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and resets the unsub status to false
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact
  remove | **string** <br /> removes the contact from the list
  unsub | **string** <br /> unsubscribes a contact from the list

<div></div>
```json
{
	"ErrorInfo": "",
	"ErrorMessage": {
		"ContactsLists": [
			{
				"ListID": "original_list_id", 
				"Action": "original_string_action", 
				"Error": "errorstring"
			}
		]
	},
	"StatusCode": 400
}

```

For this API call, there is one specific <code>HTTP 400 status</code> error condition that is worth noting:
<ul>
	<li><code>400 Object properties invalid</code>: The ID of the Contact List is missing / invalid.
</ul>

<p>
	A detailed description of the error will be sent in a standard API error payload: <br />

	The <code>errorpayload</code> will contain the following json detailing each property error in the original json payload. Only the properties having an error will be listed. <br /><br />
	<br>
	The possible <code>errorstring</code> are:
	</p>
	<ul>
		<li>listID missing or not valid</li>
		<li>action missing or not valid</li>
	</ul>

<h2 id="contact_managemanycontacts">Managing multiple contacts</h2>

```php
<?php
/*
Create : Manage the details of a Contact.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'ContactsLists' => [
        [
            'ListID' => 1,
            'action' => "addnoforce"
        ],
        [
            'ListID' => 2,
            'action' => "addforce"
        ]
    ],
    'Contacts' => [
        [
            'Email' => "jimsmith@example.com",
            'Name' => "Jim",
            'Properties' => [
                'Property1' => "value",
                'Property2' => "value2"
            ]
        ],
        [
            'Email' => "janetdoe@example.com",
            'Name' => "Janet",
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
		"ContactsLists":[
				{
						"ListID": 1,
						"action": "addnoforce"
				},
				{
						"ListID": 2,
						"action": "addforce"
				}
		],
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
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
		"ContactsLists":[
				{
						"ListID": 1,
						"action": "addnoforce"
				},
				{
						"ListID": 2,
						"action": "addforce"
				}
		],
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
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
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Contact_managemanycontacts.create(contacts_lists: [{
    'ListID'=> 1,
    'action'=> 'addnoforce'
}, {
    'ListID'=> 2,
    'action'=> 'addforce'
}],
contacts: [{
    'Email'=> 'jimsmith@example.com',
    'Name'=> 'Jim',
    'Properties'=> {
        'Property1'=> 'value',
        'Property2'=> 'value2'
    }
}, {
    'Email'=> 'janetdoe@example.com',
    'Name'=> 'Janet',
    'Properties'=> {
        'Property1'=> 'value',
        'Property2'=> 'value2'
    }
}]
)
p variable.attributes['Data']
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
  'ContactsLists': [
				{
						"ListID": 1,
						"action": "addnoforce"
				},
				{
						"ListID": 2,
						"action": "addforce"
				}
		],
  'Contacts': [
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
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
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
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
      ContactsLists: []MailjetContactsList {
        MailjetContactsList {
          ListID: 1,
          Action: "addnoforce",
        },
        MailjetContactsList {
          ListID: 2,
          Action: "addforce",
        },
      },
      Contacts: []MailjetContact {
        MailjetContact {
          Email: "jimsmith@example.com",
          Name: "Jim",
          Properties: MyPropertiesStruct {
            Property1: "value",
            Property2: "value2",
          },
        },
        MailjetContact {
          Email: "janetdoe@example.com",
          Name: "Janet",
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
			.property(ContactManagemanycontacts.CONTACTSLISTS, new JSONArray()
                .put(new JSONObject()
                    .put("ListID", "1")
                    .put("action", "addnoforce"))
                .put(new JSONObject()
                    .put("ListID", "2")
                    .put("action", "addforce")))
			.property(ContactManagemanycontacts.CONTACTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "jimsmith@example.com")
                    .put("Name", "Jim")
                    .put("Properties", new JSONObject()
                        .put("Property1", "value")
                        .put("Property2", "value2")))
                .put(new JSONObject()
                    .put("Email", "janetdoe@example.com")
                    .put("Name", "Janet")
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
            .Property(ContactManagemanycontacts.ContactsLists, new JArray {
                new JObject {
                 {"ListID", 1},
                 {"action", "addnoforce"}
                 },
                new JObject {
                 {"ListID", 2},
                 {"action", "addforce"}
                 }
                })
            .Property(ContactManagemanycontacts.Contacts, new JArray {
                new JObject {
                 {"Email", "jimsmith@example.com"},
                 {"Name", "Jim"},
                 {"Properties", new JObject {
                  {"Property1", "value"},
                  {"Property2", "value2"}
                  }}
                 },
                new JObject {
                 {"Email", "janetdoe@example.com"},
                 {"Name", "Janet"},
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Uploading multiple contacts, with the option to manage their subscription to a Contact List or multiple lists, if required, can be done with a <code>POST</code> on the <code>[/contact/managemanycontacts](/email-api/v3/contact-managemanycontacts/)</code> resource. This resource is asynchronous and will return a <code>JobID</code> allowing you to monitor the process.

<code>Email</code> is the only mandatory property in <code>Contacts</code>.

If you are specifying <code>Properties</code> for a contact, please note that these properties must already be defined with the <code>[/contactmetadata](#defining-custom-contact-data)</code> resource. The properties specified can only be <code>static</code> in this resource.

If a contact (uniquely identified by the email) has already been added, multiple entries or subsequent uploads will not add duplicate entries in neither the contacts and list subscription. The <code>Properties</code> and <code>Name</code> of the contact will be updated with any modified values.

To unassign custom contact data for specific contact, set the value of the contact property to <code>null</code>.

<code>ContactsLists</code> in this <code>POST</code> is not mandatory. You can upload multiple contact without adding them to a list. The field <code>Action</code> in ContactsLists can have the following values:

Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and re-subscribes the contact to the list
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact
  remove | **string** <br /> removes the contact from the list
  unsub | **string** <br /> unsubscribes the contact from the list

<div></div>

> API response:

```json
{
   "Count": 1,
   "Data": [
      {
         "JobID": 35800
      }
   ],
   "Total": 1
}

```

A successful call will return JSON in the following format, containing the <code>JobID</code> allowing to monitor the processing :

<div></div>
### Monitoring the upload of multiple contacts

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$ContactManagemanycontacts, ['actionID' => $id]);
$response->success() && var_dump($response->getData());
?>
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
     * View : Manage the details of a Contact.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactManagemanycontacts.resource, 'actionid')
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contact_managemanycontacts.get(action_id='$JOBID')
```
``` ruby
# View : Job information
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
end
variable = Mailjet::Contact_managemanycontacts.find($JOBID)
```
```shell
# View: Job information
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/managemanycontacts/$JOBID
```
```javascript
/**
 *
 * View: Job information
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("contact")
	.action('managemanycontacts')
	.id($JOBID)
	.request()
request
  .then(result => {
    console.log(result.body)
  })
  .catch(error => {
    console.log(error.statusCode)
  })
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
      /// View: Job information.
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
            ActionId = $JOBID
         };
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
         "Status": "string",
         "JobStart": "timestamp",
         "JobEnd": "timestamp",
         "Count": "integer",
         "Error": "string",
         "ErrorFile": "string"
      }
   ],
   "Total": 1
}
```

After the <code>POST</code> on <code>/contact/managemanycontacts</code>, we can follow with a <code>GET</code>, this time including the <code>JobID</code>

This provides the following information:

 - Status: This can be one of the following:
  - Allocated: the job is in the queue
  - Upload: The data is in the upload phase
  - Prepare: The data is being formatted to be added to the list
  - Importing: Data is being added to the Contact List
  - Completed: Addition to the Contact List complete
  - Error: Declares an error
  - Abort: For cancelled jobs.
 - JobStart: Time of job start
 - JobEnd: Time of job end
 - Count: Represents the number of contacts already processed by the background job
 - Error: If the status equals 'error', contains error information from the batch job
 - ErrorFile: Contains the URL to the Error information file

<div></div>

> Recovering error file 

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$BatchjobJsonerror, ['id' => $JobID]);
$response->success() && var_dump($response->getBody());
?>
```
```java
/**
* Not available in Java , please refer to Curl
*/
```
``` python
"""
Not available in Python, please refer to Curl
"""
```
``` ruby
# Not available in Ruby, please refer to Curl
```
```shell
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/DATA/batchjob/$JOBID/JSONError/application:json/LAST
```
```javascript
/**
 *
 * Not available in Node, please refer to Curl
 *
 */
```
``` go
/*
Not available in Go, please refer to Curl
*/
```



<h2 id="contactslist_managemanycontacts">Managing contacts in a list</h2>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Action' => "addnoforce",
    'Contacts' => [
        [
            'Email' => "jimsmith@example.com",
            'Name' => "Jim",
            'Properties' => [
                'Property1' => "value",
                'Property2' => "value2"
            ]
        ],
        [
            'Email' => "janetdoe@example.com",
            'Name' => "Janet",
            'Properties' => [
                'Property1' => "value",
                'Property2' => "value2"
            ]
        ]
    ]
];
$response = $mj->post(Resources::$ContactslistManagemanycontacts, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist/$ID/ManageManyContacts \
	-H 'Content-Type: application/json' \
	-d '{
		"Action":"addnoforce",
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
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
 * Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contactslist")
	.id($ID)
	.action("ManageManyContacts")
	.request({
		"Action":"addnoforce",
		"Contacts":[
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
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
# Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist_managemanycontacts.create(id: $ID, , action: "addnoforce",contacts: [{ 'Email'=> 'jimsmith@example.com', 'Name'=> 'Jim', 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}, { 'Email'=> 'janetdoe@example.com', 'Name'=> 'Janet', 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}])
```
```python
"""
Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'Action': 'addnoforce',
  'Contacts': [
				{
						"Email": "jimsmith@example.com",
						"Name": "Jim",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				},
				{
						"Email": "janetdoe@example.com",
						"Name": "Janet",
						"Properties": {
								"Property1": "value",
								"Property2": "value2"
						}
				}
		]
}
result = mailjet.contactslist_ManageManyContacts.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
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
	var data []resources.ContactslistManageManyContacts
	mr := &Request{
	  Resource: "contactslist",
	  ID: RESOURCE_ID,
	  Action: "ManageManyContacts",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.ContactslistManageManyContacts {
      Action: "addnoforce",
      Contacts: []MailjetContact {
        MailjetContact {
          Email: "jimsmith@example.com",
          Name: "Jim",
          Properties: MyPropertiesStruct {
            Property1: "value",
            Property2: "value2",
          },
        },
        MailjetContact {
          Email: "janetdoe@example.com",
          Name: "Janet",
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
import com.mailjet.client.resource.ContactslistManagemanycontacts;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactslistManagemanycontacts.resource, ID)
						.property(ContactslistManagemanycontacts.ACTION, "addnoforce")
						.property(ContactslistManagemanycontacts.CONTACTS, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Email", "jimsmith@example.com")
                                                                    .put("Name", "Jim")
                                                                    .put("Properties", new JSONObject()
                                                                        .put("Property1", "value")
                                                                        .put("Property2", "value2")))
                                                                .put(new JSONObject()
                                                                    .put("Email", "janetdoe@example.com")
                                                                    .put("Name", "Janet")
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
      /// Create : Manage your contact lists. One Contact might be associated to one or more ContactsList.
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
            Resource = ContactslistManagemanycontacts.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(ContactslistManagemanycontacts.Action, "addnoforce")
            .Property(ContactslistManagemanycontacts.Contacts, new JArray {
	       new JObject {
	          {"Email", "jimsmith@example.com"},
	          {"Name", "Jim"},
	          {"Properties", new JObject {
	             {"Property1", "value"},
	             {"Property2", "value2"}
	             }
                  }
	       },
	       new JObject {
	          {"Email", "janetdoe@example.com"},
	          {"Name", "Janet"},
	          {"Properties", new JObject {
	             {"Property1", "value"},
	             {"Property2", "value2"}
	             }
                  }
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


> API response:

```json
{
   "Count": 1,
   "Data": [
      {
         "JobID": 35800
      }
   ],
   "Total": 1
}

```

Multiple contacts, in JSON format, can be uploaded with a <code>POST</code> using the <code>[/contactslist/$ID/managemanycontacts](/email-api/v3/contactslist-managemanycontacts/)</code> action. This resource is asynchronous and will return a <code>JobID</code> allowing you to monitor the process. This is the perfect method to easily add large quantity of contacts in a list.

The field <code>Email</code> in Contacts is the key for the contact and is mandatory, as is <code>Action</code>. <code>Action</code> can have one of the following values:

Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and re-subscribes the contact to the list
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact
  remove | **string** <br /> removes the contact from the List
  unsub | **string** <br /> unsubscribes the contact from the List

If you are specifying <code>Properties</code> for a contact, please note that these properties must already be defined with the <code>[/contactmetadata](#defining-custom-contact-data)</code> resource. The properties specified can only be <code>static</code> in this resource. To unassign custom contact data for specific contact, set the value of the contact property to <code>null</code>.

If a contact (uniquely identified by the email) has already been added to your contacts or a list, multiple entries or subsequent uploads will not add duplicate entries in either the contacts or list subscription. The <code>Properties</code> and <code>Name</code> of the contact will be updated with any modified values.

<div></div>
### Monitoring the upload of multiple contacts

```php
<?php
// View: Job information
require 'vendor/autoload.php';
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$result = $mj->get(Resources::$ContactslistManagemanycontacts, ['id' => $id, 'actionid' => $actionId]);
var_dump($result->getData());
?>
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactslist;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : only need a Name
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactslistManagemanycontacts.resource, 'id', 'jobid');
      response = client.get(request);
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactslist_managemanycontacts.get(id='$ID', actionid='$JOBID')
```
```shell
# View: Job information
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist/$ID/ManageManyContacts/$JOBID
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
end
variable = Mailjet::Contactslist_managemanycontacts.find($ID, $JOBID)
```
```javascript
/**
 *
 * View: Job information
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("contact")
	.id($ID)
	.action('managemanycontacts')
	.id($JOBID)
	.request()
request
  .then(result => {
    console.log(result.body)
  })
  .catch(error => {
    console.log(error.statusCode)
  })
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
   /// View: Job information.
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
            Resource = ContactslistManagemanycontacts.Resource,
            ResourceId = $ID,
            ActionId = $JOBID
         };
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
         "Status": "string",
         "JobStart": "timestamp",
         "JobEnd": "timestamp",
         "Count": "integer",
         "Error": "string",
         "ErrorFile": "string"
      }
   ],
   "Total": 1
}
```


After the <code>POST</code> on <code>/contactslist/$ID/managemanycontacts</code>, we can follow with a <code>GET</code>, this time including the <code>JobID</code>

This provides the following information:

 - Status: This can be one of the following:
  - Allocated: the job is in the queue
  - Upload: The data is in the upload phase
  - Prepare: The data is being formatted to be added to the list
  - Importing: Data is being added to the Contact List
  - Completed: Addition to the Contact List complete
  - Error: Declares an error
  - Abort: For cancelled jobs.
 - JobStart: Time of job start
 - JobEnd: Time of job end
 - Count: Represents the number of contacts already processed by the background job
 - Error: If the status equals 'error', contains error information from the batch job
 - ErrorFile: Contains the URL to the Error information file

> Recovering error file 

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$BatchjobJsonerror, ['id' => $JobID]);
$response->success() && var_dump($response->getBody());
?>
```
```java
/**
* Not available in Java , please refer to Curl
*/
```
``` python
"""
Not available in Python, please refer to Curl
"""
```
``` ruby
# Not available in Ruby, please refer to Curl
```
```shell
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/DATA/batchjob/$JOBID/JSONError/application:json/LAST
```
```javascript
/**
 *
 * Not available in Node, please refer to Curl
 *
 */
```
``` go
/*
Not available in Go, please refer to Curl
*/
```


<h2 id="csv_upload_contacts">Contacts CSV upload</h2>

In some cases you might need to manage large quantities of contacts stored into a csv record in relation to a contactslist. 

With the following process, you will be able to fully manage your contacts and their relationship with a contact list (ie: adding, removing or unsubscribing)

Please note that these steps represent a single process. Don't execute each step independently but rather as a whole.

###CSV file structure

```
"email","age"
"foo@example.org",42
"bar@example.com",13
"sam@ple.co.uk",37
```

The structure for the csv file should be as follows:

You can define Custom Contact Data in the csv. Visit our [Contact Personalisation Guide](#personalisation-add-contact-properties) for more information about defining Custom Contact Data. If you are using undefined contact properties, Mailjet will automatically create <code>/contactmetadata</code> in the next step of the process.

With this process it is not possible to populate the Contact Name property. If you specify a "name" it will create a new <code>/contactmetadata</code> and <code>/contactdata</code> as explained above.
The email should be unique in the file and will be the key to reconcile this list with your existing contact in Mailjet system. 

<div></div>
###Uploading of the data

``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
f = open('./data.csv')
result = mailjet.contactslist_csvdata.create(id='$ID_CONTACTLIST', data=f.read())
print result.status_code
print result.json()
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
     * Modify : A wrapper for the CSV importer
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Csvimport.resource, ID)
        .property(Csvimport.DATA, MYFILE);
      response = client.put(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
// View: CSV upload
$CSVContent = file_get_contents('./test.csv');
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$ContactslistCsvdata, ['body' => $CSVContent, 'id' => $ID_CONTACTLIST]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Import the CSV file through the DATA API
curl --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v3/DATA/contactslist/$ID_CONTACTLIST/CSVData/text:plain \
-H "Content-Type: text/plain" \
--data-binary "@./test.csv"
```
``` ruby
# unsupported for now
```
``` javascript
const fs = require ('fs')
const mailjet = require ('node-mailjet')
    .connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
fs.readFile('./data.csv', (err, data) => {
  if (err) { return console.error(err) }
  const request = mailjet.post('contactslist')
      .id("$ID_CONTACTLIST")
      .action('csvdata')
      .request(data);
  request
  .then(result => {
    console.log(result.body)
  })
  .catch(error => {
    console.log(error.statusCode)
  })
})
```
``` go
package main
import (
    "fmt"
    "io/ioutil"
    . "github.com/mailjet/mailjet-apiv3-go"
    "os"
)
func check(e error) {
    if e != nil {
        panic(e)
    }
}
func main () {
    dat, err := ioutil.ReadFile("./data.csv")
    check(err)
    mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
    type MyStruct struct {
	   ID int
    }
    var data = new(MyStruct)
    mr := &DataRequest{
      SourceType: "contactslist",
      SourceTypeID: "$ID_CONTACTLIST",
      DataType: "CSVData",
      MimeType: "text:plain",
    }
    fmr := &FullDataRequest{
      Info: mr,
      Payload: string(dat),
    }
    err1 := mailjetClient.PostData(fmr, &data)
    if err1 != nil {
      fmt.Println(err1)
    }
    fmt.Printf("Data array: %+v\n", data)
}
```


> API response:

```json 
{
   "ID": 57
}
```

The first step is to upload the csv data to the server.
You need to specify the wanted <code>contactslist</code> ID and, of course, the csv_content.

<div></div>
###Adding the contacts and subscriptions to the contact list

```php
<?php
/*
Create: A wrapper for the CSV importer
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'ContactsListID' => "$ID_CONTACTLIST",
    'DataID' => "$ID_DATA",
    'Method' => "addnoforce"
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
		"ContactsListID":"$ID_CONTACTLIST",
		"DataID":"$ID_DATA",
		"Method":"addnoforce"
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
		"ContactsListID":"$ID_CONTACTLIST",
		"DataID":"$ID_DATA",
		"Method":"addnoforce"
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
variable = Mailjet::Csvimport.create(contacts_list_id: "$ID_CONTACTLIST",
data_id: "$ID_DATA",
method: "addnoforce"
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
  'ContactsListID': '$ID_CONTACTLIST',
  'DataID': '$ID_DATA',
  'Method': 'addnoforce'
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
      ContactsListID: "$ID_CONTACTLIST",
      DataID: "$ID_DATA",
      Method: "addnoforce",
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
			.property(Csvimport.CONTACTSLISTID, "$ID_CONTACTLIST")
			.property(Csvimport.DATAID, "$ID_DATA")
			.property(Csvimport.METHOD, "addnoforce");
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
            .Property(Csvimport.ContactsListID, "$ID_CONTACTLIST")
            .Property(Csvimport.DataID, "$ID_DATA")
            .Property(Csvimport.Method, "addnoforce");
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"AliveAt": "",
			"ContactsListID": "",
			"Count": "1",
			"Current": "",
			"DataID": "",
			"Errcount": "0",
			"ErrTreshold": "",
			"ID": "",
			"ImportOptions": "",
			"JobEnd": "",
			"JobStart": "",
			"Method": "",
			"RequestAt": "",
			"Status": "Upload"
		}
	],
	"Total": 1
}
```


Now, the uploaded data needs to be assigned to the given <code>contactslist</code> resource.

Method's possible values are: 

  Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and resets the unsub status to false
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact
  remove | **string** <br /> removes the contact from the List
  unsub | **string** <br /> unsubscribes a contact from the List

<div></div>
###Monitoring the process

```shell
# View: CSV upload Batch job running on the Mailjet infrastructure.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/csvimport/$ID_JOB 
```
```php
<?php
/*
View: CSV upload Batch job running on the Mailjet infrastructure.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Csvimport, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * View: CSV upload Batch job running on the Mailjet infrastructure.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("csvimport")
	.id($ID_JOB)
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
# View: CSV upload Batch job running on the Mailjet infrastructure.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Csvimport.find($ID_JOB)
p variable.attributes['Data']
```
```python
"""
View: CSV upload Batch job running on the Mailjet infrastructure.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_JOB'
result = mailjet.csvimport.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View: CSV upload Batch job running on the Mailjet infrastructure.
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
import com.mailjet.client.resource.Csvimport;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View: CSV upload Batch job running on the Mailjet infrastructure.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Csvimport.resource, ID);
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
      /// View: CSV upload Batch job running on the Mailjet infrastructure.
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"AliveAt": "",
			"ContactsList": "",
			"Count": "1",
			"Current": "",
			"DataID": "",
			"Errcount": "0",
			"ErrTreshold": "",
			"ID": "",
			"ImportOptions": "",
			"JobEnd": "",
			"JobStart": "",
			"Method": "",
			"RequestAt": "",
			"Status": "Completed"
		}
	],
	"Total": 1
}
```


You can now make sure the task completed successfully. You might need multiple checks as a huge amount of data may take some time to be processed (several hours are not uncommon).
Using the job <code>ID</code> returned in the previous step, you can retrieve the job status

	This provides the following information:
	<ul>
		<li>Status: This can be one of the following:
			<ul>
			    <li>Prepare: The data is being formatted to be added to the list</li>
			    <li>Importing: Data is being added to the Contact List</li>
			    <li>Completed: Addition to the Contact List complete</li>
			    <li>Error: Declares an error</li>
			</ul>
		</li>
		<li>JobStart: Time of job start</li>
		<li>JobEnd: Time of job end</li>
		<li>Count: Represents the number of contacts already processed by the background job</li>
		<li>Errcount: contains the number of errors detected during the processus, the Status can be "Completed" and have errors</li>
	</ul>

<div></div>
###Error handling

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$BatchjobCsverror, ['id' => $job_id]);
$response->success() && var_dump($response->getBody());
?>
```
```java
/**
* Not available in Java , please refer to Curl
*/
```
``` python
"""
Not available in Python, please refer to Curl
"""
```
``` ruby
# Not available in Ruby, please refer to Curl
```
```shell
curl --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v3/DATA/BatchJob/$job_id/CSVError/text:csv
```
```javascript
/**
 *
 * Not available in Node, please refer to Curl
 *
 */
```
``` go
/*
Not available in Go, please refer to Curl
*/
```


Using the job ID, you can retrieve the error file (if any, see <code>Errcount</code> number in the response of the monitoring of the job), through the DATA API. This error file will give you a textual reason for errors.

<div></div>
The returned file will be a copy of your original file with an added column describing the error for each line in error. 

> File uploaded with error : 

```
"email","age"
"foo@example.org",42
"bar@example.com"
"sam@ple.co.uk",37
```

> Error file content

```
email,age,error
"bar@example.com", ###Too few columns at line
```
