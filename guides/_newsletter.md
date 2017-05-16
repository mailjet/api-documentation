#Send marketing campaigns

To send your first newsletter, you need to have at least one active sender address in the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section.

You will need to create contacts and organize them into lists.
To do so, you have two options : use the <a href="https://app.mailjet.com/contacts/lists" target="_blank">interface</a> or the API.

## Create a contact list

```php
<?php
/*
Create : only need a Name
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Name' => "myList"
];
$response = $mj->post(Resources::$Contactslist, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : only need a Name
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist \
	-H 'Content-Type: application/json' \
	-d '{
		"Name":"myList"
	}'
```
```javascript
/**
 *
 * Create : only need a Name
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contactslist")
	.request({
		"Name":"myList"
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
# Create : only need a Name
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist.create(name: "myList")
```
```python
"""
Create : only need a Name
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Name': 'myList'
}
result = mailjet.contactslist.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : only need a Name
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
	var data []resources.Contactslist
	mr := &Request{
	  Resource: "contactslist",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contactslist {
      Name: "myList",
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
      request = new MailjetRequest(Contactslist.resource)
						.property(Contactslist.NAME, "myList");
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
      /// Create : only need a Name
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
            Resource = Contactslist.Resource,
         }
            .Property(Contactslist.Name, "myList");
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
			"Address": "",
			"CreatedAt": "",
			"ID": "",
			"IsDeleted": "false",
			"Name": "myList",
			"SubscriberCount": ""
		}
	],
	"Total": 1
}
```


You can create your contacts list by performing a simple <code>POST</code> request on the <code>[/contactslist](/email-api/v3/contactslist/)</code> resource with only one mandatory field : its name.

The <code>Name</code> will be a unique identifier for your list. 

<div></div>

## Manage contacts

### Create a contact

```php
<?php
/*
Create : Manage the details of a Contact.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Email' => "Mister@mailjet.com"
];
$response = $mj->post(Resources::$Contact, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Manage the details of a Contact.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact \
	-H 'Content-Type: application/json' \
	-d '{
		"Email":"Mister@mailjet.com"
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
	.request({
		"Email":"Mister@mailjet.com"
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
variable = Mailjet::Contact.create(email: "Mister@mailjet.com")
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
  'Email': 'Mister@mailjet.com'
}
result = mailjet.contact.create(data=data)
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
	var data []resources.Contact
	mr := &Request{
	  Resource: "contact",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contact {
      Email: "Mister@mailjet.com",
    },
	}
	err := mailjetClient.Post(fmr, &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
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
            Resource = Contact.Resource,
         }
            .Property(Contact.Email, "Mister@mailjet.com");
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
     * Create : Manage the details of a Contact.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contact.resource)
						.property(Contact.EMAIL, "Mister@mailjet.com");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
 

> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "",
			"DeliveredCount": "",
			"Email": "Mister@mailjet.com",
			"ExclusionFromCampaignsUpdatedAt": "",
			"ID": "",
			"IsExcludedFromCampaigns": "false",
			"IsOptInPending": "false",
			"IsSpamComplaining": "false",
			"LastActivityAt": "",
			"LastUpdateAt": "",
			"Name": "MisterMailjet",
			"UnsubscribedAt": "",
			"UnsubscribedBy": ""
		}
	],
	"Total": 1
}
```


To create some contacts which will be the recipients, you need to specify an email address with <code>POST</code> on the <code>[/contact](/email-api/v3/contact/)</code> resource. 

The email address will be a unique identifier of your contact in the Mailjet System. 

<div></div>

### Personalisation : add contact properties

The <code>/contact</code> resource allows you to create contacts using their email addresses and names. If you want to add more granular details about your contacts, Mailjet provides the capability to add custom data to contacts. 

The addition of custom data starts with the definition of the extra information to store with the contacts (It could be for example the country the contacts live in, how old the contacts are, their current income, the value of their purchases on your site...) and how this data will be stored (string, number, boolean...). 

#### Defining custom Contact data

```php
<?php
/*
Create : Definition of available extra data items for contacts.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Datatype' => "str",
    'Name' => "Age",
    'NameSpace' => "static"
];
$response = $mj->post(Resources::$Contactmetadata, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Definition of available extra data items for contacts.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactmetadata \
	-H 'Content-Type: application/json' \
	-d '{
		"Datatype":"str",
		"Name":"Age",
		"NameSpace":"static"
	}'
```
```javascript
/**
 *
 * Create : Definition of available extra data items for contacts.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contactmetadata")
	.request({
		"Datatype":"str",
		"Name":"Age",
		"NameSpace":"static"
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
# Create : Definition of available extra data items for contacts.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactmetadata.create(datatype: "str",name: "Age",name_space: "static")
```
```python
"""
Create : Definition of available extra data items for contacts.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Datatype': 'str',
  'Name': 'Age',
  'NameSpace': 'static'
}
result = mailjet.contactmetadata.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Definition of available extra data items for contacts.
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
	var data []resources.Contactmetadata
	mr := &Request{
	  Resource: "contactmetadata",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contactmetadata {
      Datatype: "str",
      Name: "Age",
      NameSpace: "static",
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
import com.mailjet.client.resource.Contactmetadata;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Definition of available extra data items for contacts.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contactmetadata.resource)
						.property(Contactmetadata.DATATYPE, "str")
						.property(Contactmetadata.NAME, "Age")
						.property(Contactmetadata.NAMESPACE, "static");
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
      /// Create : Definition of available extra data items for contacts.
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
            Resource = Contactmetadata.Resource,
         }
            .Property(Contactmetadata.Datatype, "str")
            .Property(Contactmetadata.Name, "Age")
            .Property(Contactmetadata.NameSpace, "static");
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
			"Datatype": "str",
			"ID": "",
			"Name": "Age",
			"NameSpace": "static"
		}
	],
	"Total": 1
}
```


To define custom contact data, perform a <code>POST</code> on <code>[/contactmetadata](/email-api/v3/contactmetadata/)</code> with the following properties:

 - <code>Name</code>: the name of the custom data field
 - <code>DataType</code>: the type of data that is being stored (this can be either a <code>str</code>, <code>int</code>, <code>float</code> or <code>bool</code>)
 - <code>NameSpace</code>: this can be either <code>static</code> or <code>historic</code>

For example, to store the age of each contacts, a <code>static</code> <code>int</code> "Age" property can be added to the metadata.

A <code>static</code> data stores only one value per DataType. It could store for example a firstname, a lastname, a country, a language ...  
A <code>historic</code> data stores a timestamped history of the value of this data. It could be used for example to store the value of each purchases over the history of the contact. 

These 2 Namespaces have their own resources for viewing, creating and editing: <code>[/contactdata](/email-api/v3/contactdata/)</code> and <code>[/contacthistorydata](/email-api/v3/contacthistorydata/)</code>  

The contact datas will be available for personalisation of your message content and for segmentation of your lists.

<div></div>
#### Adding custom static Contact data

```bash
# Modify : Modify the static custom contact data
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactdata/$CONTACT_ID \
	-H 'Content-Type: application/json' \
	-d '{
		"Data":[
				{
						"Name": "Age",
						"value": 30
				},
				{
						"Name": "Country",
						"value": "US"
				}
		]
	}'
```
```php
<?php
/*
Modify : Modify the static custom contact data
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Data' => [
        [
            'Name' => "Age",
            'value' => 30
        ],
        [
            'Name' => "Country",
            'value' => "US"
        ]
    ]
];
$response = $mj->put(Resources::$Contactdata, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * Modify : Modify the static custom contact data
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.put("contactdata")
	.id($CONTACT_ID)
	.request({
		"Data":[
				{
						"Name": "Age",
						"value": 30
				},
				{
						"Name": "Country",
						"value": "US"
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
# Modify : Modify the static custom contact data
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Contactdata.find($CONTACT_ID)
target.update_attributes(data: [{ 'Name'=> 'Age', 'value'=> 30}, { 'Name'=> 'Country', 'value'=> 'US'}])
```
```python
"""
Modify : Modify the static custom contact data
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$CONTACT_ID'
data = {
  'Data': [
				{
						"Name": "Age",
						"value": 30
				},
				{
						"Name": "Country",
						"value": "US"
				}
		]
}
result = mailjet.contactdata.update(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Modify : Modify the static custom contact data
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
	mr := &Request{
	  Resource: "contactdata",
	  ID: RESOURCE_ID,
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contactdata {
      Data: []MailjetDat {
        MailjetDat {
          Name: "Age",
          Value: 30,
        },
        MailjetDat {
          Name: "Country",
          Value: "US",
        },
      },
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
import com.mailjet.client.resource.Contactdata;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Modify : Modify the static custom contact data
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contactdata.resource, ID)
						.property(Contactdata.DATA, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Name", "Age")
                                                                    .put("value", "30"))
                                                                .put(new JSONObject()
                                                                    .put("Name", "Country")
                                                                    .put("value", "US")));
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
      /// Modify : Modify the static custom contact data
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
            Resource = Contactdata.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(Contactdata.Data, new JArray {
                new JObject {
                 {"Name", "Age"},
                 {"value", "30"}
                 },
                new JObject {
                 {"Name", "Country"},
                 {"value", "US"}
                 }
                });
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
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


By Performing a <code>PUT</code> (acting like a PATCH, your other static datas will not be lost) on <code>[/contactdata](/email-api/v3/contactdata/)</code>, you can add values for several metadata at once.

<div></div>
#### Adding custom historic Contact data

```php
<?php
/*
Create : This resource can be used to add historical data to contact.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'ContactID' => "$CONTACT_ID",
    'Data' => 10,
    'Name' => "Purchase"
];
$response = $mj->post(Resources::$Contacthistorydata, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : This resource can be used to add historical data to contact.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contacthistorydata \
	-H 'Content-Type: application/json' \
	-d '{
		"ContactID":"$CONTACT_ID",
		"Data":"10",
		"Name":"Purchase"
	}'
```
```javascript
/**
 *
 * Create : This resource can be used to add historical data to contact.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contacthistorydata")
	.request({
		"ContactID":"$CONTACT_ID",
		"Data":"10",
		"Name":"Purchase"
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
# Create : This resource can be used to add historical data to contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contacthistorydata.create(contact_id: "$CONTACT_ID",data: "10",name: "Purchase")
```
```python
"""
Create : This resource can be used to add historical data to contact.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'ContactID': '$CONTACT_ID',
  'Data': '10',
  'Name': 'Purchase'
}
result = mailjet.contacthistorydata.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : This resource can be used to add historical data to contact.
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
	var data []resources.Contacthistorydata
	mr := &Request{
	  Resource: "contacthistorydata",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contacthistorydata {
      ContactID: "$CONTACT_ID",
      Data: 10,
      Name: "Purchase",
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
import com.mailjet.client.resource.Contacthistorydata;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : This resource can be used to add historical data to contact.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contacthistorydata.resource)
						.property(Contacthistorydata.CONTACTID, "$CONTACT_ID")
						.property(Contacthistorydata.DATA, "10")
						.property(Contacthistorydata.NAME, "Purchase");
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
      /// Create : This resource can be used to add historical data to contact.
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
            Resource = Contacthistorydata.Resource,
         }
            .Property(Contacthistorydata.ContactID, "$CONTACT_ID")
            .Property(Contacthistorydata.Data, "10")
            .Property(Contacthistorydata.Name, "Purchase");
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


By Performing a <code>POST</code> on <code>[/contacthistorydata](/email-api/v3/contacthistorydata/)</code>, you can add a new value for a historic metadata.

This historic metadata will be available for segmentation with dedicated functions like <code>Avg</code>,<code>Min</code>,<code>Max</code>...

<div></div>
#### Creating contacts with Contact data  

```php
<?php
/*
Create : Manage the details of a Contact.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
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
```bash
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact_managemanycontacts.create(contacts: [{ 'Email'=> 'jimsmith@example.com', 'Name'=> 'Jim', 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}, { 'Email'=> 'janetdoe@example.com', 'Name'=> 'Janet', 'Properties'=> { 'Property1'=> 'value', 'Property2'=> 'value2' }}])
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
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Mailjet offers the <code>[/contact/managemanycontacts](/email-api/v3/contact-managemanycontacts/)</code> resource to create or update multiple contacts and their properties at once.
 
[More information](#contact_managemanycontacts)

<div></div>
### Subscribe to a list

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
```bash
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact_managecontactslists.create(id: $ID, contacts_lists: [{ 'ListID'=> '$ListID_1', 'Action'=> 'addnoforce'}, { 'ListID'=> '$ListID_2', 'Action'=> 'addforce'}])
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
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```
``` go
/*
Create : Manage a contact subscription to a list
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


Last step: you have to add this contact to the contactslist previously created.

<code>POST</code> on the <code>[/contact/$ID/managecontactslists](/email-api/v3/contact-managecontactslists/)</code> with $ID being the Mailjet contact id or the email address of the contact. 
You can specify one or more list to add this contact to. The <code>action</code> specified with the ListID can be <code>addforce</code> (force the subscription of the contact even if he unsubscribed to the list before) or <code>addnoforce</code> (do not resubscribe the contact to the list if unsubscribed before) 

<code>managecontactslist</code> offer more possibilities of list management for a contact. [More information](#managecontactslists)

<div></div>
### Create contact and subscribe at once

```php
<?php
/*
Add a contact to the list
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Email' => "mrsmith@mailjet.com",
    'Name' => "MrSmith",
    'Action' => "addnoforce",
    'Properties' => [
        'property1' => "value",
        'propertyN' => "valueN"
    ]
];
$response = $mj->post(Resources::$ContactslistManagecontact, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Add a contact to the list
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist/$LIST_ID/managecontact \
	-H 'Content-Type: application/json' \
	-d '{
		"Email":"mrsmith@mailjet.com",
		"Name":"MrSmith",
		"Action":"addnoforce",
		"Properties":{
				"property1": "value",
				"propertyN": "valueN"
		}
	}'
```
```javascript
/**
 *
 * Add a contact to the list
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contactslist")
	.id($LIST_ID)
	.action("managecontact")
	.request({
		"Email":"mrsmith@mailjet.com",
		"Name":"MrSmith",
		"Action":"addnoforce",
		"Properties":{
				"property1": "value",
				"propertyN": "valueN"
		}
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
# Add a contact to the list
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist_managecontact.create(id: $LIST_ID, , , , email: "mrsmith@mailjet.com",name: "MrSmith",action: "addnoforce",properties: { 'property1'=> 'value', 'propertyN'=> 'valueN'})
```
```python
"""
Add a contact to the list
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$LIST_ID'
data = {
  'Email': 'mrsmith@mailjet.com',
  'Name': 'MrSmith',
  'Action': 'addnoforce',
  'Properties': {
				"property1": "value",
				"propertyN": "valueN"
		}
}
result = mailjet.contactslist_managecontact.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Add a contact to the list
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
	"os"
)
type  MyPropertiesStruct  struct {
  Property1  string
  PropertyN  string
}
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	var data []resources.ContactslistManagecontact
	mr := &Request{
	  Resource: "contactslist",
	  ID: RESOURCE_ID,
	  Action: "managecontact",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.ContactslistManagecontact {
      Email: "mrsmith@mailjet.com",
      Name: "MrSmith",
      Action: "addnoforce",
      Properties: MyPropertiesStruct {
        Property1: "value",
        PropertyN: "valueN",
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
import com.mailjet.client.resource.ContactslistManagecontact;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Add a contact to the list
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(ContactslistManagecontact.resource, ID)
						.property(ContactslistManagecontact.EMAIL, "mrsmith@mailjet.com")
						.property(ContactslistManagecontact.NAME, "MrSmith")
						.property(ContactslistManagecontact.ACTION, "addnoforce")
						.property(ContactslistManagecontact.PROPERTIES, new JSONObject()
                                                                .put("property1", "value")
                                                                .put("propertyN", "valueN"));
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
      /// Add a contact to the list
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
            Resource = ContactslistManagecontact.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(ContactslistManagecontact.Email, "mrsmith@mailjet.com")
            .Property(ContactslistManagecontact.Name, "MrSmith")
            .Property(ContactslistManagecontact.Action, "addnoforce")
            .Property(ContactslistManagecontact.Properties, new JObject {
                {"property1", "value"},
                {"propertyN", "valueN"}
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
			"ContactID": "10",
			"Email": "mrsmith@mailjet.com",
			"Action": "addnoforce",
			"Name": "MrSmith",
			"Properties": {
				"property1": "value",
				"propertyN": "valueN"
			}
		}
	],
	"Total": 1
}
```


The <code>[/contactslist/$ID/managecontact](/email-api/v3/contactslist-managecontact/)</code> allows to create and add a contact to a list in one call. 

The properties specified in this call can only be static Contact data. The Contact 

  'Action' is mandatory and can be one of the following values:

  Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and resets the unsub status to false
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact
  remove | **string** <br /> removes the contact from the list
  unsub | **string** <br /> unsubscribes a contact from the list

The response will include the <code>ID</code> of the contact that was created or updated. 

<aside class="notice">
All properties used must be defined in advance in the user interface or through the resource <code>/contactmetadata</code> 
</aside>

<div></div>


> Error response:

```json
{
    "ErrorInfo": {
        "Properties": [
            {
                "property1": "Property not defined"
            },
            {
                "propertyN": "Property type does not match definition"
            }
        ]
    },
    "ErrorMessage": "Object properties invalid",
    "StatusCode": 400
}
```

In case of error, <code>/contactslist/$ID/managecontact</code> will return JSON payload containing a <code>ErrorInfo</code> and <code>ErrorMessage</code>.


## Add Contacts in bulks

###Managing and uploading multiple contacts ( /contact/managemanycontacts ) 

This resource allows to add contacts in bulk in a json format. Optionally, these contacts can be added to existing lists. 
[More information](#contact_managemanycontacts)

###Managing multiple Contacts subscriptions to a List ( /contactslist/$ID/managemanycontacts ) 

This resource allows to add contacts directly to a list. This resource will create new contacts if the contacts are not already in the Mailjet system. 
[More information](#contactslist_managemanycontacts)


###Managing Contacts through CSV upload

This process allows to upload csv containing large quantities of contacts.
[More information](#csv_upload_contacts)

<div id="prepare-a-newsletter"></div>
<div id="create-a-newsletter"></div>
##Prepare a campaign

### Create a campaign draft

```php
<?php
/*
Create : CampaignDraft data
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Locale' => "en_US",
    'Sender' => "MisterMailjet",
    'SenderEmail' => "Mister@mailjet.com",
    'Subject' => "Greetings from Mailjet",
    'ContactsListID' => "$ID_CONTACTSLIST",
    'Title' => "Friday newsletter"
];
$response = $mj->post(Resources::$Campaigndraft, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : CampaignDraft data
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft \
	-H 'Content-Type: application/json' \
	-d '{
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTSLIST",
		"Title":"Friday newsletter"
	}'
```
```javascript
/**
 *
 * Create : CampaignDraft data
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.request({
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTSLIST",
		"Title":"Friday newsletter"
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
# Create : CampaignDraft data
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft.create(locale: "en_US",sender: "MisterMailjet",sender_email: "Mister@mailjet.com",subject: "Greetings from Mailjet",contacts_list_id: "$ID_CONTACTSLIST",title: "Friday newsletter")
```
```python
"""
Create : CampaignDraft data
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Locale': 'en_US',
  'Sender': 'MisterMailjet',
  'SenderEmail': 'Mister@mailjet.com',
  'Subject': 'Greetings from Mailjet',
  'ContactsListID': '$ID_CONTACTSLIST',
  'Title': 'Friday newsletter'
}
result = mailjet.campaigndraft.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : CampaignDraft data
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
	var data []resources.Campaigndraft
	mr := &Request{
	  Resource: "campaigndraft",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Campaigndraft {
      Locale: "en_US",
      Sender: "MisterMailjet",
      SenderEmail: "Mister@mailjet.com",
      Subject: "Greetings from Mailjet",
      ContactsListID: "$ID_CONTACTSLIST",
      Title: "Friday newsletter",
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
import com.mailjet.client.resource.Campaigndraft;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : CampaignDraft data
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Campaigndraft.resource)
						.property(Campaigndraft.LOCALE, "en_US")
						.property(Campaigndraft.SENDER, "MisterMailjet")
						.property(Campaigndraft.SENDEREMAIL, "Mister@mailjet.com")
						.property(Campaigndraft.SUBJECT, "Greetings from Mailjet")
						.property(Campaigndraft.CONTACTSLISTID, "$ID_CONTACTSLIST")
						.property(Campaigndraft.TITLE, "Friday newsletter");
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
      /// Create : CampaignDraft data
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
            Resource = Campaigndraft.Resource,
         }
            .Property(Campaigndraft.Locale, "en_US")
            .Property(Campaigndraft.Sender, "MisterMailjet")
            .Property(Campaigndraft.SenderEmail, "Mister@mailjet.com")
            .Property(Campaigndraft.Subject, "Greetings from Mailjet")
            .Property(Campaigndraft.ContactsListID, "$ID_CONTACTSLIST")
            .Property(Campaigndraft.Title, "Friday newsletter");
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
			"AXFractionName": "",
			"AXTesting": "",
			"CampaignID": "",
			"ContactsListID": "$ID_CONTACTSLIST",
			"CreatedAt": "",
			"Current": "",
			"DeliveredAt": "",
			"EditMode": "",
			"ID": "",
			"IsStarred": "false",
			"IsTextPartIncluded": "false",
			"Locale": "en_US",
			"ModifiedAt": "",
			"Preset": "",
			"ReplyEmail": "",
			"SegmentationID": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"Template": "",
			"Title": "Friday newsletter",
			"Url": "",
			"Used": "false"
		}
	],
	"Total": 1
}
```


To create a campaign draft, perform a POST on the <code>[/campaigndraft](/email-api/v3/campaigndraft/)</code> resource. Required fields are a <code>Locale</code>, <code>Sender</code>, <code>SenderEmail</code>, <code>Subject</code> and <code>ContactsListID</code>.

Reminder: the SenderEmail needs to be active. Visit the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section to check.

<div></div>
### Add a body to a campaign draft

```php
<?php
/*
Modify : CampaignDraft content data.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Html-part' => "Hello <strong>world</strong>!",
    'Text-part' => "Hello world!"
];
$response = $mj->post(Resources::$CampaigndraftDetailcontent, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Modify : CampaignDraft content data.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft/$ID/detailcontent \
	-H 'Content-Type: application/json' \
	-d '{
		"Html-part":"Hello <strong>world</strong>!",
		"Text-part":"Hello world!"
	}'
```
```javascript
/**
 *
 * Modify : CampaignDraft content data.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.id($ID)
	.action("detailcontent")
	.request({
		"Html-part":"Hello <strong>world</strong>!",
		"Text-part":"Hello world!"
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
# Modify : CampaignDraft content data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft_detailcontent.create(id: $ID, , html_part: "Hello <strong>world</strong>!",text_part: "Hello world!")
```
```python
"""
Modify : CampaignDraft content data.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'Html-part': 'Hello <strong>world</strong>!',
  'Text-part': 'Hello world!'
}
result = mailjet.campaigndraft_detailcontent.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Modify : CampaignDraft content data.
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
	var data []resources.CampaigndraftDetailcontent
	mr := &Request{
	  Resource: "campaigndraft",
	  ID: RESOURCE_ID,
	  Action: "detailcontent",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.CampaigndraftDetailcontent {
      HtmlPart: "Hello <strong>world</strong>!",
      TextPart: "Hello world!",
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
import com.mailjet.client.resource.CampaigndraftDetailcontent;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Modify : CampaignDraft content data.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(CampaigndraftDetailcontent.resource, ID)
						.property(CampaigndraftDetailcontent.HTMLPART, "Hello <strong>world</strong>!")
						.property(CampaigndraftDetailcontent.TEXTPART, "Hello world!");
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
      /// Modify : CampaignDraft content data.
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
            Resource = CampaigndraftDetailcontent.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(CampaigndraftDetailcontent.Htmlpart, "Hello <strong>world</strong>!")
            .Property(CampaigndraftDetailcontent.Textpart, "Hello world!");
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


>API response : 

```json
{
	"Count": 1,
	"Data": [
		{
			"Html-part": "Hello <strong>world</strong>!",
			"Text-part": "Hello world!"
		}
	],
	"Total": 1
}
```

Now that we have a campaign draft, we can add the most important property: its content, which can be Text or Html (Text-part or Html-part). To do so, you can perform a <code>PUT</code> or <code>POST</code> request  on the <code>[/campaigndraft/$ID/detailcontent](/email-api/v3/campaigndraft-detailcontent/)</code> resource.
With a <code>POST</code>, if you only give a value to Html-part or Text-part, the other one will be set to empty.
While with a <code>PUT</code>, if you only give a value to Html-part or Text-part, the other one keeps its previous value.

<aside class="notice">
Any update of the <code>MJMLContent</code> property of the <code>/campaigndraft/$ID/detailcontent</code> resource will not automaticaly generate the <code>Html-part</code> version of the content. In this case, you should regenerate the <code>Html-part</code> with the MJML tools.
</aside>

<div id="send-a-newsletter"></div>
<div id="test-newsletters"></div>
## Send a campaign

### Test a campaign draft

```php
<?php
/*
Create : CampaignDraft test
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Recipients' => [
        [
            'Email' => "mailjet@example.org",
            'Name' => "Mailjet"
        ]
    ]
];
$response = $mj->post(Resources::$CampaigndraftTest, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : CampaignDraft test
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft/$ID/test \
	-H 'Content-Type: application/json' \
	-d '{
		"Recipients":[
				{
						"Email": "mailjet@example.org",
						"Name": "Mailjet"
				}
		]
	}'
```
```javascript
/**
 *
 * Create : CampaignDraft test
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.id($ID)
	.action("test")
	.request({
		"Recipients":[
				{
						"Email": "mailjet@example.org",
						"Name": "Mailjet"
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
# Create : CampaignDraft test
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft_test.create(id: $ID, recipients: [{ 'Email'=> 'mailjet@example.org', 'Name'=> 'Mailjet'}])
```
```python
"""
Create : CampaignDraft test
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'Recipients': [
				{
						"Email": "mailjet@example.org",
						"Name": "Mailjet"
				}
		]
}
result = mailjet.campaigndraft_test.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : CampaignDraft test
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
	var data []resources.CampaigndraftTest
	mr := &Request{
	  Resource: "campaigndraft",
	  ID: RESOURCE_ID,
	  Action: "test",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.CampaigndraftTest {
      Recipients: []MailjetRecipient {
        MailjetRecipient {
          Email: "mailjet@example.org",
          Name: "Mailjet",
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
import com.mailjet.client.resource.CampaigndraftTest;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : CampaignDraft test
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(CampaigndraftTest.resource, ID)
						.property(CampaigndraftTest.RECIPIENTS, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Email", "mailjet@example.org")
                                                                    .put("Name", "Mailjet")));
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
      /// Create : CampaignDraft test
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
            Resource = CampaigndraftTest.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(CampaigndraftTest.Recipients, new JArray {
                new JObject {
                 {"Email", "mailjet@example.org"},
                 {"Name", "Mailjet"}
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
			"Status": "Draft"
		}
	],
	"Total": 1
}
```


To send a test version of a draft, you need to perform a POST request on the resource <code>[/campaigndraft/$ID/test](/email-api/v3/campaigndraft-test/)</code> with a recipient in the JSON packet.

<aside class="notice">
Before sending, the API will check if the draft has all mandatory fields filled in and that they are valid. In case of error, the API will return a 400 Bad Request.
</aside>

<div></div>

<div id="send-the-newsletter-immediately"></div>
### Send the campaign immediately

```php
<?php
/*
Send a CampaignDraft
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$CampaigndraftSend, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Send a CampaignDraft
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft/$ID/send \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Send a CampaignDraft
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.id($ID)
	.action("send")
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
# Send a CampaignDraft
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft_send.create(id: $ID)
```
```python
"""
Send a CampaignDraft
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.campaigndraft_send.create(id=id)
print result.status_code
print result.json()
```
``` go
/*
Send a CampaignDraft
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
	var data []resources.CampaigndraftSend
	mr := &Request{
	  Resource: "campaigndraft",
	  ID: RESOURCE_ID,
	  Action: "send",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.CampaigndraftSend {
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
import com.mailjet.client.resource.CampaigndraftSend;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Send a CampaignDraft
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(CampaigndraftSend.resource, ID);
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
      /// Send a CampaignDraft
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
            Resource = CampaigndraftSend.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
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
			"Status": "programmed"
		}
	],
	"Total": 1
}
```


Once the campaign draft is completely set up, it can be sent with a <code>POST</code> on the <code>[/campaigndraft/$ID/send](/email-api/v3/campaigndraft-send/)</code> resource.

Before sending, the API will check if the draft has all mandatory fields filled in and that they are valid :

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this draft


<div></div>
<div id="schedule-a-newsletter"></div>
### Schedule a campaign

> Schedule a camp for the 22nd of April, 2015 at 9am UTC+1

```php
<?php
/*
Create : CampaignDraft schedule
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'date' => "2015-04-22T09:00:00+01:00"
];
$response = $mj->post(Resources::$CampaigndraftSchedule, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : CampaignDraft schedule
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft/$ID/schedule \
	-H 'Content-Type: application/json' \
	-d '{
		"date":"2015-04-22T09:00:00+01:00"
	}'
```
```javascript
/**
 *
 * Create : CampaignDraft schedule
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.id($ID)
	.action("schedule")
	.request({
		"date":"2015-04-22T09:00:00+01:00"
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
# Create : CampaignDraft schedule
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft_schedule.create(id: $ID, date: "2015-04-22T09:00:00+01:00")
```
```python
"""
Create : CampaignDraft schedule
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'date': '2015-04-22T09:00:00+01:00'
}
result = mailjet.campaigndraft_schedule.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : CampaignDraft schedule
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
	var data []resources.CampaigndraftSchedule
	mr := &Request{
	  Resource: "campaigndraft",
	  ID: RESOURCE_ID,
	  Action: "schedule",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.CampaigndraftSchedule {
      Date: "2015-04-22T09:00:00+01:00",
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
import com.mailjet.client.resource.CampaigndraftSchedule;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : CampaignDraft schedule
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(CampaigndraftSchedule.resource, ID)
						.property(CampaigndraftSchedule.DATE, "2015-04-22T09:00:00+01:00");
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
      /// Create : CampaignDraft schedule
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
            Resource = CampaigndraftSchedule.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(CampaigndraftSchedule.date, "2015-04-22T09:00:00+01:00");
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
			"Status": "programmed"
		}
	],
	"Total": 1
}
```


To send a campaign at a later date, we need to set the date property (ISO 8601 format) at which the campaign shall be sent using the <code>[/campaigndraft/$ID/schedule](/email-api/v3/campaigndraft-schedule/)</code> resource.

Before scheduling, the API will check if the campaign draft has all mandatory fields filled in and that they are valid:

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this campaign draft
 - Date of scheduling is not in the past or too close to the execution

The value <code>NOW</code> is accepted as a date to indicate immediate sending.

In order to cancel scheduled campaign, you should use <code>DELETE</code> action on <code>[/campaigndraft/$ID/schedule](/email-api/v3/campaigndraft-schedule/)</code> resource.

<div id="checking-a-newsletter-status"></div>
### Checking a campaign draft status

```php
<?php
/*
View : CampaignDraft data.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Campaigndraft, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : CampaignDraft data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft/$ID 
```
```javascript
/**
 *
 * View : CampaignDraft data.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("campaigndraft")
	.id($ID)
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
# View : CampaignDraft data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft.find($ID)
```
```python
"""
View : CampaignDraft data.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.campaigndraft.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : CampaignDraft data.
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
	var data []resources.Campaigndraft
	mr := &Request{
	  Resource: "campaigndraft",
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
import com.mailjet.client.resource.Campaigndraft;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : CampaignDraft data.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Campaigndraft.resource, ID);
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
      /// View : CampaignDraft data.
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
            Resource = Campaigndraft.Resource,
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
			"AXFractionName": "",
			"AXTesting": "",
			"CampaignID": "",
			"ContactsListID": "$ID_CONTACTSLIST",
			"CreatedAt": "",
			"Current": "",
			"DeliveredAt": "",
			"EditMode": "",
			"ID": "",
			"IsStarred": "false",
			"IsTextPartIncluded": "false",
			"Locale": "en_US",
			"ModifiedAt": "",
			"Preset": "",
			"ReplyEmail": "",
			"SegmentationID": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"Template": "",
			"Title": "Friday newsletter",
			"Url": "",
			"Used": "false"
		}
	],
	"Total": 1
}
```


Using a GET on the <code>[/campaigndraft](/email-api/v3/campaigndraft/)</code> resource, you can find the <code>Status</code> of your draft. 

The status can have the following value: 

 - <code>-2</code> : deleted
 - <code>-1</code> : archived draft
 - <code>0</code> : draft
 - <code>1</code> : programmed
 - <code>2</code> : sent
 - <code>3</code> : A/X tesring


##Segmentation

Our <code>[/contactfilter](/email-api/v3/contactfilter/)</code> resource allows you to send campaigns to certain subsets of your contact lists. A filter can be applied to any contact metadata that you defined, like age, gender, and location.

###Prerequisites

In order to use a contact filter, you must add additional data to your contacts. Please refer to the [personalisation](#personalisation-add-contact-properties) to see how this is done. You also have to create a contact list, because contact filters are applied to a contact list via the <code>/campaigndraft</code> resource.

###How does it work?

Mailjet allows you to segment a list of contacts by creating a ContactFilter resource. This resource has a property called expression and it is the value of this property that is used to filter a list of contacts. You can create simple expressions using the <code>=</code>, <code><</code>, <code>></code> and <code>!=</code> operators:

- age>40
- gender=male
- country=France

You can also apply the negative statement <code>not</code>

But you can also combine these operators with the <code>and</code> and <code>or</code> operator, using brackets:

- (age>40) and (gender=male)
- (country=France) and (profession=physician)
- ((age>=50) and (age<70)) or (income>50000)


Additionaly, you can filter on the contact Activities (who has opened/clicked your campaigns in the last X days). These function return a boolean.

- <code>HasOpenedSince(days)</code> 
- <code>HasClickedSince(days)</code> 

These functions are not scoped on a specific campaign. 

<div></div>
###Create a contact filter

```php
<?php
/*
Create : A filter expressions for use in newsletters.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Description' => "Only contacts aged 40",
    'Expression' => "age=40",
    'Name' => "40 year olds"
];
$response = $mj->post(Resources::$Contactfilter, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : A filter expressions for use in newsletters.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactfilter \
	-H 'Content-Type: application/json' \
	-d '{
		"Description":"Only contacts aged 40",
		"Expression":"age=40",
		"Name":"40 year olds"
	}'
```
```javascript
/**
 *
 * Create : A filter expressions for use in newsletters.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("contactfilter")
	.request({
		"Description":"Only contacts aged 40",
		"Expression":"age=40",
		"Name":"40 year olds"
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
# Create : A filter expressions for use in newsletters.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactfilter.create(description: "Only contacts aged 40",expression: "age=40",name: "40 year olds")
```
```python
"""
Create : A filter expressions for use in newsletters.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Description': 'Only contacts aged 40',
  'Expression': 'age=40',
  'Name': '40 year olds'
}
result = mailjet.contactfilter.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : A filter expressions for use in newsletters.
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
	var data []resources.Contactfilter
	mr := &Request{
	  Resource: "contactfilter",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Contactfilter {
      Description: "Only contacts aged 40",
      Expression: "age=40",
      Name: "40 year olds",
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
import com.mailjet.client.resource.Contactfilter;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : A filter expressions for use in newsletters.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Contactfilter.resource)
						.property(Contactfilter.DESCRIPTION, "Only contacts aged 40")
						.property(Contactfilter.EXPRESSION, "age=40")
						.property(Contactfilter.NAME, "40 year olds");
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
      /// Create : A filter expressions for use in newsletters.
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
            Resource = Contactfilter.Resource,
         }
            .Property(Contactfilter.Description, "Only contacts aged 40")
            .Property(Contactfilter.Expression, "age=40")
            .Property(Contactfilter.Name, "40 year olds");
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
			"Description": "Only contacts contacts aged 40",
			"Expression": "age=40",
			"ID": "",
			"Name": "40 year olds",
			"Status": "unused"
		}
	],
	"Total": 1
}
```


Lets say that we added an age property to our contacts, using the <code>[/contactmetadata](/email-api/v3/contactmetadata/)</code> and <code>[/contactdata](/email-api/v3/contactdata/)</code> resources. 
We now want to create a filter that gives us only those contacts that are 40 years old. In order to do this, we perform a <code>POST</code> with the following properties: <code>Name</code> , <code>Expression</code> , and <code>Description</code> . Name and Description allow you to describe the filter,while <code>Expression</code> contains the actual filtering expression.

<div></div>
###Create a campaign with a segmentation filter

```php
<?php
/*
Create : CampaignDraft data
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Locale' => "en_US",
    'Sender' => "MisterMailjet",
    'SenderEmail' => "Mister@mailjet.com",
    'Subject' => "Greetings from Mailjet",
    'ContactsListID' => "$ID_CONTACTSLIST",
    'Title' => "Friday newsletter",
    'SegmentationID' => "$ID_CONTACT_FILTER"
];
$response = $mj->post(Resources::$Campaigndraft, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : CampaignDraft data
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaigndraft \
	-H 'Content-Type: application/json' \
	-d '{
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTSLIST",
		"Title":"Friday newsletter",
		"SegmentationID":"$ID_CONTACT_FILTER"
	}'
```
```javascript
/**
 *
 * Create : CampaignDraft data
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("campaigndraft")
	.request({
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTSLIST",
		"Title":"Friday newsletter",
		"SegmentationID":"$ID_CONTACT_FILTER"
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
# Create : CampaignDraft data
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaigndraft.create(locale: "en_US",sender: "MisterMailjet",sender_email: "Mister@mailjet.com",subject: "Greetings from Mailjet",contacts_list_id: "$ID_CONTACTSLIST",title: "Friday newsletter",segmentation_id: "$ID_CONTACT_FILTER")
```
```python
"""
Create : CampaignDraft data
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Locale': 'en_US',
  'Sender': 'MisterMailjet',
  'SenderEmail': 'Mister@mailjet.com',
  'Subject': 'Greetings from Mailjet',
  'ContactsListID': '$ID_CONTACTSLIST',
  'Title': 'Friday newsletter',
  'SegmentationID': '$ID_CONTACT_FILTER'
}
result = mailjet.campaigndraft.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : CampaignDraft data
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
	var data []resources.Campaigndraft
	mr := &Request{
	  Resource: "campaigndraft",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Campaigndraft {
      Locale: "en_US",
      Sender: "MisterMailjet",
      SenderEmail: "Mister@mailjet.com",
      Subject: "Greetings from Mailjet",
      ContactsListID: "$ID_CONTACTSLIST",
      Title: "Friday newsletter",
      SegmentationID: "$ID_CONTACT_FILTER",
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
import com.mailjet.client.resource.Campaigndraft;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : CampaignDraft data
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Campaigndraft.resource)
						.property(Campaigndraft.LOCALE, "en_US")
						.property(Campaigndraft.SENDER, "MisterMailjet")
						.property(Campaigndraft.SENDEREMAIL, "Mister@mailjet.com")
						.property(Campaigndraft.SUBJECT, "Greetings from Mailjet")
						.property(Campaigndraft.CONTACTSLISTID, "$ID_CONTACTSLIST")
						.property(Campaigndraft.TITLE, "Friday newsletter")
						.property(Campaigndraft.SEGMENTATIONID, "$ID_CONTACT_FILTER");
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
      /// Create : CampaignDraft data
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
            Resource = Campaigndraft.Resource,
         }
            .Property(Campaigndraft.Locale, "en_US")
            .Property(Campaigndraft.Sender, "MisterMailjet")
            .Property(Campaigndraft.SenderEmail, "Mister@mailjet.com")
            .Property(Campaigndraft.Subject, "Greetings from Mailjet")
            .Property(Campaigndraft.ContactsListID, "$ID_CONTACTSLIST")
            .Property(Campaigndraft.Title, "Friday newsletter")
            .Property(Campaigndraft.SegmentationID, "$ID_CONTACT_FILTER");
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


Segmentation is achieved by adding a contact filter to a campaign draft resource using the property <code>SegmentationID</code>. <code>$ID_CONTACT_FILTER</code> is the ID of the contact filter that was created on the previous step.

##Campaign and Statistics

A new campaign resource is created for each campaign draft and transactional email sent.  You can query the campaign resource and its related statistics resources for a variety of data like bounces, number of clicks, and sending time.

###Retrieve campaign information for a particular newsletter

```php
<?php
/*
View : Campaign linked to the Newsletter :NEWSLETTER_ID
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Campaign, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Campaign linked to the Newsletter :NEWSLETTER_ID
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaign/mj.nl=$NEWSLETTER_ID 
```
```javascript
/**
 *
 * View : Campaign linked to the Newsletter :NEWSLETTER_ID
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("campaign")
	.id(mj.nl=$NEWSLETTER_ID)
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
# View : Campaign linked to the Newsletter :NEWSLETTER_ID
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaign.find(mj.nl=$NEWSLETTER_ID)
```
```python
"""
View : Campaign linked to the Newsletter :NEWSLETTER_ID
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = 'mj.nl=$NEWSLETTER_ID'
result = mailjet.campaign.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : Campaign linked to the Newsletter :NEWSLETTER_ID
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
	var data []resources.Campaign
	mr := &Request{
	  Resource: "campaign",
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
import com.mailjet.client.resource.Campaign;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Campaign linked to the Newsletter :NEWSLETTER_ID
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Campaign.resource, ID);
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
      /// View : Campaign linked to the Newsletter :NEWSLETTER_ID
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
            Resource = Campaign.Resource,
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
			"CampaignType": "",
			"ClickTracked": "",
			"CreatedAt": "",
			"CustomValue": "",
			"FirstMessageID": "",
			"FromID": "",
			"FromEmail": "miss@mailjet.com",
			"FromName": "",
			"HasHtmlCount": "",
			"HasTxtCount": "",
			"ID": "",
			"IsDeleted": "false",
			"IsStarred": "false",
			"ListID": "",
			"NewsLetterID": "",
			"OpenTracked": "",
			"SegmentationID": "",
			"SendEndAt": "",
			"SendStartAt": "",
			"SpamassScore": "",
			"Status": "",
			"Subject": "",
			"UnsubscribeTrackedCount": ""
		}
	],
	"Total": 1
}
```


When a campaign is created from the processing of a newsletter, its <code>CustomValue</code> property is set to <code>mj.nl=$NEWSLETTER_ID</code> where <code>$NEWSLETTER_ID</code> is the id of the newsletter you just sent. You can use <code>mj.nl=$NEWSLETTER_ID</code> as a unique key to retrieve the campaign.


<div></div>
### Campaign statistics

```php
<?php
/*
View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Campaignstatistics);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaignstatistics 
```
```javascript
/**
 *
 * View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("campaignstatistics")
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
# View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Campaignstatistics.all()
```
```python
"""
View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.campaignstatistics.get()
print result.status_code
print result.json()
```
``` go
/*
View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
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
	var data []resources.Campaignstatistics
	_, _, err := mailjetClient.List("campaignstatistics", &data)
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
import com.mailjet.client.resource.Campaignstatistics;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Campaignstatistics.resource);
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
      /// View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
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
            Resource = Campaignstatistics.Resource,
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
			"AXTesting": "",
			"BlockedCount": "",
			"BouncedCount": "",
			"CampaignID": "",
			"CampaignIsStarred": "false",
			"CampaignSendStartAt": "",
			"CampaignSubject": "",
			"ClickedCount": "",
			"ContactListName": "",
			"DeliveredCount": "",
			"LastActivityAt": "",
			"NewsLetterID": "",
			"OpenedCount": "",
			"ProcessedCount": "",
			"QueuedCount": "",
			"SegmentName": "",
			"SpamComplaintCount": "",
			"UnsubscribedCount": ""
		}
	],
	"Total": 1
}
```


View message statistics grouped by campaigns using <code>[/campaignstatistics](/email-api/v3/campaignstatistics/)</code> resource .

The [API reference](/email-api/v3/campaign/) offers more information on additional resources to explore the campaigns.

##API Legacy : /newsletter 

<code>[/campaigndraft](/email-api/v3/campaigndraft/)</code> replaced <code>[/newsletter](/email-api/v3/newsletter/)</code> for the management and configuration of campaign draft.

<code>/campaigndraft</code> is used to access all draft created with Passport and through the <code>/campaigndraft</code> API resource.

<code>/newsletter</code> can still be used to access all draft created with Mailjet old builder and through the <code>/newsletter</code> API resource.

<code>/campaigndraft</code> will not allow access to any of the draft created with <code>/newsletter</code>

If you have been using the <code>/newsletter</code> API resource to create campaign drafts, make sure to continue to use the <code>/newsletter</code> resource to access them.

 







 

