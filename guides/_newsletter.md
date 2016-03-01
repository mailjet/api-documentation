#Send marketing campaigns

To send your first newsletter, you need to have at least one active sender address in the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section.

You will need to create contacts and organize them into lists.
To do so, you have two options : use the <a href="https://app.mailjet.com/contacts/lists" target="_blank">interface</a> or the API.

## Create a contact list

```php
<?php
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("contactslist")
	.request({
		"Name":"myList"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contactslist",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactslist;
public class MyClass {
    /**
     * Create : only need a Name
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Contactslist.resource)
						.property(Contactslist.NAME, "myList");
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("contact")
	.request({
		"Email":"Mister@mailjet.com"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contact",
	}
	fmr := &FullMailjetRequest{
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contact;
public class MyClass {
    /**
     * Create : Manage the details of a Contact.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("contactmetadata")
	.request({
		"Datatype":"str",
		"Name":"Age",
		"NameSpace":"static"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contactmetadata",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactmetadata;
public class MyClass {
    /**
     * Create : Definition of available extra data items for contacts.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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

```php
<?php
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
```javascript
/**
 *
 * Modify : Modify the static custom contact data
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
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
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contactdata",
	  ID: RESOURCE_ID,
	}
	fmr := &FullMailjetRequest{
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactdata;
public class MyClass {
    /**
     * Modify : Modify the static custom contact data
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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


By Performing a <code>PUT</code> (acting like a PATCH, your other static datas will not be lost) on <code>[/contactdata](/email-api/v3/contactdata/)</code>, you can add values for several metadata at once.

<div></div>
#### Adding custom historic Contact data

```php
<?php
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("contacthistorydata")
	.request({
		"ContactID":"$CONTACT_ID",
		"Data":"10",
		"Name":"Purchase"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contacthistorydata",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contacthistorydata;
public class MyClass {
    /**
     * Create : This resource can be used to add historical data to contact.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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


By Performing a <code>POST</code> on <code>[/contacthistorydata](/email-api/v3/contacthistorydata/)</code>, you can add a new value for a historic metadata.

This historic metadata will be available for segmentation with dedicated functions like <code>Avg</code>,<code>Min</code>,<code>Max</code>...

<div></div>
#### Creating contacts with Contact data  

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
```javascript
/**
 *
 * Create : Manage the details of a Contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
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
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contact",
	  Action: "managemanycontacts",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.ContactManagemanycontacts;
public class MyClass {
    /**
     * Create : Manage the details of a Contact.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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


Mailjet offers the <code>[/contact/managemanycontacts](/email-api/v3/contact-managemanycontacts/)</code> resource to create or update multiple contacts and their properties at once.
 
[More information](#contact_managemanycontacts)

<div></div>
### Subscribe to a list

```php
<?php
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
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
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.ContactManagecontactslists;
public class MyClass {
    /**
     * Create : Manage a contact subscription to a list
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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
	mr := &MailjetRequest{
	  Resource: "contact",
	  ID: RESOURCE_ID,
	  Action: "managecontactslists",
	}
	fmr := &FullMailjetRequest{
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


Last step: you have to add this contact to the contactslist previously created.

<code>POST</code> on the <code>[/contact/$ID/managecontactslists](/email-api/v3/contact-managecontactslists/)</code> with $ID being the Mailjet contact id or the email address of the contact. 
You can specify one or more list to add this contact to. The <code>action</code> specified with the ListID can be <code>addforce</code> (force the subscription of the contact even if he unsubscribed to the list before) or <code>addnoforce</code> (do not resubscribe the contact to the list if unsubscribed before) 

<code>managecontactslist</code> offer more possibilities of list management for a contact. [More information](#managecontactslists)

<div></div>
### Create contact and subscribe at once

```php
<?php
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
```javascript
/**
 *
 * Add a contact to the list
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
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
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contactslist",
	  ID: RESOURCE_ID,
	  Action: "managecontact",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.ContactslistManagecontact;
public class MyClass {
    /**
     * Add a contact to the list
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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

The properties specified in this call can only be static Contact data.

  'Action' is mandatory and can be one of the following values:

  Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and resets the unsub status to false
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact

The response will include the <code>ID</code> of the contact that was created or updated. 

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

##Prepare a newsletter

### Create a Newsletter

```php
<?php
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
$response = $mj->post(Resources::$Newsletter, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Newsletter data.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter \
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
 * Create : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("newsletter")
	.request({
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTSLIST",
		"Title":"Friday newsletter"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter.create(locale: "en_US",sender: "MisterMailjet",sender_email: "Mister@mailjet.com",subject: "Greetings from Mailjet",contacts_list_id: "$ID_CONTACTSLIST",title: "Friday newsletter")
```
```python
"""
Create : Newsletter data.
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
result = mailjet.newsletter.create(data=data)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Newsletter;
public class MyClass {
    /**
     * Create : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Newsletter.resource)
						.property(Newsletter.LOCALE, "en_US")
						.property(Newsletter.SENDER, "MisterMailjet")
						.property(Newsletter.SENDEREMAIL, "Mister@mailjet.com")
						.property(Newsletter.SUBJECT, "Greetings from Mailjet")
						.property(Newsletter.CONTACTSLISTID, "$ID_CONTACTSLIST")
						.property(Newsletter.TITLE, "Friday newsletter");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
Create : Newsletter data.
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
	var data []resources.Newsletter
	mr := &MailjetRequest{
	  Resource: "newsletter",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.Newsletter {
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


> API response: 

```json
{
	"Count": 1,
	"Data": [
		{
			"AXFraction": "",
			"AXFractionName": "",
			"AXTesting": "",
			"Callback": "",
			"Campaign": "",
			"ContactsList": "",
			"CreatedAt": "",
			"DeliveredAt": "",
			"EditMode": "",
			"EditType": "",
			"Footer": "",
			"FooterAddress": "",
			"FooterWYSIWYGType": "",
			"HeaderFilename": "",
			"HeaderLink": "",
			"HeaderText": "",
			"HeaderUrl": "",
			"ID": "",
			"Ip": "",
			"IsHandled": "false",
			"IsStarred": "false",
			"IsTextPartIncluded": "false",
			"Locale": "en_US",
			"ModifiedAt": "",
			"Permalink": "",
			"PermalinkHost": "",
			"PermalinkWYSIWYGType": "",
			"PolitenessMode": "",
			"ReplyEmail": "",
			"Segmentation": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"Template": "",
			"TestAddress": "",
			"Title": "",
			"Url": ""
		}
	],
	"Total": 1
}
```


To create a newsletter, perform a POST on the <code>[/newsletter](/email-api/v3/newsletter/)</code> resource. Required fields are a <code>Locale</code>, <code>Sender</code>, <code>SenderEmail</code>, <code>Subject</code> and <code>ContactsListID</code>.

Reminder: the SenderEmail needs to be active. Visit the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section to check.

<div></div>
### Add a body to a newsletter

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Html-part' => "Hello <strong>world</strong>!",
    'Text-part' => "Hello world!"
];
$response = $mj->put(Resources::$NewsletterDetailcontent, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Modify : Newsletter data.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID/detailcontent \
	-H 'Content-Type: application/json' \
	-d '{
		"Html-part":"Hello <strong>world</strong>!",
		"Text-part":"Hello world!"
	}'
```
```javascript
/**
 *
 * Modify : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("newsletter")
	.id($ID)
	.action("detailcontent")
	.request({
		"Html-part":"Hello <strong>world</strong>!",
		"Text-part":"Hello world!"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Modify : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Newsletter_detailcontent.find($ID)
target.update_attributes(html_part: "Hello <strong>world</strong>!",text_part: "Hello world!")
```
```python
"""
Modify : Newsletter data.
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
result = mailjet.newsletter_detailcontent.update(id=id, data=data)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.NewsletterDetailcontent;
public class MyClass {
    /**
     * Modify : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(NewsletterDetailcontent.resource, ID)
						.property(NewsletterDetailcontent.HTMLPART, "Hello <strong>world</strong>!")
						.property(NewsletterDetailcontent.TEXTPART, "Hello world!");
      response = client.put(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
Modify : Newsletter data.
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
	mr := &MailjetRequest{
	  Resource: "newsletter",
	  ID: RESOURCE_ID,
	  Action: "detailcontent",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.NewsletterDetailcontent {
      HtmlPart: "Hello <strong>world</strong>!",
      TextPart: "Hello world!",
    },
	}
	err := mailjetClient.Put(fmr)
	if err != nil {
	  fmt.Println(err)
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

Now that we have a newsletter, we can add the most important property: its content, which can be Text or Html (Text-part or Html-part). To do so, you can perform a <code>PUT</code> or <code>POST</code> request  on the <code>[/newsletter/$ID/detailcontent](/email-api/v3/newsletter-detailcontent/)</code> resource.
With a <code>POST</code>, if you only give a value to Html-part or Text-part, the other one will be set to empty.
While with a <code>PUT</code>, if you only give a value to Html-part or Text-part, the other one keeps its previous value.

## Send a newsletter

### Test Newsletters

```php
<?php
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
$response = $mj->post(Resources::$NewsletterTest, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Newsletter data.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID/test \
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
 * Create : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("newsletter")
	.id($ID)
	.action("test")
	.request({
		"Recipients":[
				{
						"Email": "mailjet@example.org",
						"Name": "Mailjet"
				}
		]
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter_test.create(id: $ID, recipients: [{ 'Email'=> 'mailjet@example.org', 'Name'=> 'Mailjet'}])
```
```python
"""
Create : Newsletter data.
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
result = mailjet.newsletter_test.create(id=id, data=data)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.NewsletterTest;
public class MyClass {
    /**
     * Create : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(NewsletterTest.resource, ID)
						.property(NewsletterTest.RECIPIENTS, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Email", "mailjet@example.org")
                                                                    .put("Name", "Mailjet")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
Create : Newsletter data.
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
	var data []resources.NewsletterTest
	mr := &MailjetRequest{
	  Resource: "newsletter",
	  ID: RESOURCE_ID,
	  Action: "test",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.NewsletterTest {
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


To send a test version of a newsletter, you need to perform a POST request on the resource <code>[/newsletter/$ID/test](/email-api/v3/newsletter-test/)</code> with a recipient in the JSON packet.

<aside class="notice">
Before sending, the API will check if the newsletter has all mandatory fields filled in and that they are valid. In case of error, the API will return a 400 Bad Request.
</aside>

<div></div>

### Send the newsletter immediately

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$NewsletterSend, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Newsletter data.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID/send \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Create : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("newsletter")
	.id($ID)
	.action("send")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter_send.create(id: $ID)
```
```python
"""
Create : Newsletter data.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.newsletter_send.create(id=id)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.NewsletterSend;
public class MyClass {
    /**
     * Create : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(NewsletterSend.resource, ID);
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
Create : Newsletter data.
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
	var data []resources.NewsletterSend
	mr := &MailjetRequest{
	  Resource: "newsletter",
	  ID: RESOURCE_ID,
	  Action: "send",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.NewsletterSend {
    },
	}
	err := mailjetClient.Post(fmr, &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
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


Once the newsletter is completely set up, it can be sent with a <code>POST</code> on the <code>[/newsletter/$ID/send](/email-api/v3/newsletter-send/)</code> resource.

Before sending, the API will check if the newsletter has all mandatory fields filled in and that they are valid :

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this newsletter


<div></div>

### Schedule a newsletter

> Schedule a newsletter for the 22nd of April, 2015 at 9am UTC+1

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'date' => "2015-04-22T09:00:00+01:00"
];
$response = $mj->post(Resources::$NewsletterSchedule, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Newsletter data.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID/schedule \
	-H 'Content-Type: application/json' \
	-d '{
		"date":"2015-04-22T09:00:00+01:00"
	}'
```
```javascript
/**
 *
 * Create : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("newsletter")
	.id($ID)
	.action("schedule")
	.request({
		"date":"2015-04-22T09:00:00+01:00"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter_schedule.create(id: $ID, date: "2015-04-22T09:00:00+01:00")
```
```python
"""
Create : Newsletter data.
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
result = mailjet.newsletter_schedule.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Newsletter data.
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
	var data []resources.NewsletterSchedule
	mr := &MailjetRequest{
	  Resource: "newsletter",
	  ID: RESOURCE_ID,
	  Action: "schedule",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.NewsletterSchedule {
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.NewsletterSchedule;
public class MyClass {
    /**
     * Create : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(NewsletterSchedule.resource, ID)
						.property(NewsletterSchedule.DATE, "2015-04-22T09:00:00+01:00");
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
			"Status": "programmed"
		}
	],
	"Total": 1
}
```


To send a newsletter at a later date, we need to set the date property (ISO 8601 format) at which the newsletter shall be sent usind the <code>[/newsletter/$ID/schedule](/email-api/v3/newsletter-schedule/)</code> resource.

Before scheduling, the API will check if the newsletter has all mandatory fields filled in and that they are valid:

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this newsletter
 - Date of scheduling is not in the past or too close to the execution

The value <code>NOW</code> is accepted as a date to indicate immediate sending

<div></div>
### Checking a newsletter status

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Newsletter, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Newsletter data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID 
```
```ruby
# View : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter.find($ID)
```
```javascript
/**
 *
 * View : Newsletter data.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("newsletter")
	.id($ID)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```python
"""
View : Newsletter data.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.newsletter.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : Newsletter data.
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
	var data []resources.Newsletter
	mr := &MailjetRequest{
	  Resource: "newsletter",
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Newsletter;
public class MyClass {
    /**
     * View : Newsletter data.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Newsletter.resource, ID);
      response = client.get(request);
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
			"AXFraction": "",
			"AXFractionName": "",
			"AXTesting": "",
			"Callback": "",
			"Campaign": "",
			"ContactsList": "",
			"CreatedAt": "",
			"DeliveredAt": "",
			"EditMode": "",
			"EditType": "",
			"Footer": "",
			"FooterAddress": "",
			"FooterWYSIWYGType": "",
			"HeaderFilename": "",
			"HeaderLink": "",
			"HeaderText": "",
			"HeaderUrl": "",
			"ID": "",
			"Ip": "",
			"IsHandled": "false",
			"IsStarred": "false",
			"IsTextPartIncluded": "false",
			"Locale": "en_US",
			"ModifiedAt": "",
			"Permalink": "",
			"PermalinkHost": "",
			"PermalinkWYSIWYGType": "",
			"PolitenessMode": "",
			"ReplyEmail": "",
			"Segmentation": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"Template": "",
			"TestAddress": "",
			"Title": "",
			"Url": ""
		}
	],
	"Total": 1
}
```


Using a GET on the <code>[/newsletter](/email-api/v3/newsletter/)</code> resource, you can find the <code>Status</code> of your newsletter. 

The status can have the following value: 

 - <code>-2</code> : deleted
 - <code>-1</code> : archived draft
 - <code>0</code> : draft
 - <code>1</code> : programmed
 - <code>2</code> : sent
 - <code>3</code> : A/X tesring


##Segmentation

Our <code>[/contactfilter](/email-api/v3/contactfilter/)</code> resource allows you to send newsletters to certain subsets of your contact lists. A filter can be applied to any contact metadata that you defined, like age, gender, and location.

###Prerequisites

In order to use a contact filter, you must add additional data to your contacts. Please refer to the [personalisation](#personalisation-add-contact-properties) to see how this is done. You also have to create a contact list, because contact filters are applied to a contact list via the <code>/newsletter</code> resource.

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
```javascript
/**
 *
 * Create : A filter expressions for use in newsletters.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("contactfilter")
	.request({
		"Description":"Only contacts aged 40",
		"Expression":"age=40",
		"Name":"40 year olds"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
	  Resource: "contactfilter",
	}
	fmr := &FullMailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactfilter;
public class MyClass {
    /**
     * Create : A filter expressions for use in newsletters.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
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
###Create a newsletter with a segmentation filter

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Title' => "Mailjet greets every contact over 40",
    'Locale' => "en_US",
    'Sender' => "MisterMailjet",
    'SenderEmail' => "Mister@mailjet.com",
    'Subject' => "Greetings from Mailjet",
    'ContactsListID' => "$ID_CONTACTLIST",
    'SegmentationID' => "$ID_CONTACT_FILTER"
];
$response = $mj->post(Resources::$Newsletter, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Create : Newsletter data with segmentation.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter \
	-H 'Content-Type: application/json' \
	-d '{
		"Title":"Mailjet greets every contact over 40",
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTLIST",
		"SegmentationID":"$ID_CONTACT_FILTER"
	}'
```
```javascript
/**
 *
 * Create : Newsletter data with segmentation.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("newsletter")
	.request({
		"Title":"Mailjet greets every contact over 40",
		"Locale":"en_US",
		"Sender":"MisterMailjet",
		"SenderEmail":"Mister@mailjet.com",
		"Subject":"Greetings from Mailjet",
		"ContactsListID":"$ID_CONTACTLIST",
		"SegmentationID":"$ID_CONTACT_FILTER"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : Newsletter data with segmentation.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter.create(title: "Mailjet greets every contact over 40",locale: "en_US",sender: "MisterMailjet",sender_email: "Mister@mailjet.com",subject: "Greetings from Mailjet",contacts_list_id: "$ID_CONTACTLIST",segmentation_id: "$ID_CONTACT_FILTER")
```
```python
"""
Create : Newsletter data with segmentation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Title': 'Mailjet greets every contact over 40',
  'Locale': 'en_US',
  'Sender': 'MisterMailjet',
  'SenderEmail': 'Mister@mailjet.com',
  'Subject': 'Greetings from Mailjet',
  'ContactsListID': '$ID_CONTACTLIST',
  'SegmentationID': '$ID_CONTACT_FILTER'
}
result = mailjet.newsletter.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Newsletter data with segmentation.
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
	var data []resources.Newsletter
	mr := &MailjetRequest{
	  Resource: "newsletter",
	}
	fmr := &FullMailjetRequest{
	  Info: mr,
	  Payload: &resources.Newsletter {
      Title: "Mailjet greets every contact over 40",
      Locale: "en_US",
      Sender: "MisterMailjet",
      SenderEmail: "Mister@mailjet.com",
      Subject: "Greetings from Mailjet",
      ContactsListID: "$ID_CONTACTLIST",
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Newsletter;
public class MyClass {
    /**
     * Create : Newsletter data with segmentation.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Newsletter.resource)
						.property(Newsletter.TITLE, "Mailjet greets every contact over 40")
						.property(Newsletter.LOCALE, "en_US")
						.property(Newsletter.SENDER, "MisterMailjet")
						.property(Newsletter.SENDEREMAIL, "Mister@mailjet.com")
						.property(Newsletter.SUBJECT, "Greetings from Mailjet")
						.property(Newsletter.CONTACTSLISTID, "$ID_CONTACTLIST")
						.property(Newsletter.SEGMENTATIONID, "$ID_CONTACT_FILTER");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```


Segmentation is achieved by adding a contact filter to a newsletter resource using the property <code>SegmentationID</code>. <code>$ID_CONTACT_FILTER</code> is the ID of the contact filter that was created on the previous step.

##Campaign and Statistics

A new campaign resource is created for each newsletter and transactional email that is sent.  You can query the campaign resource and its related statistics resources for a variety of data like bounces, number of clicks, and sending time.

###Retrieve campaign information for a particular newsletter

```bash
# View : Campaign linked to the Newsletter :NEWSLETTER_ID
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/campaign/mj.nl=$NEWSLETTER_ID 
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Campaign, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * View : Campaign linked to the Newsletter :NEWSLETTER_ID
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("campaign")
	.id(mj.nl=$NEWSLETTER_ID)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
	mr := &MailjetRequest{
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Campaign;
public class MyClass {
    /**
     * View : Campaign linked to the Newsletter :NEWSLETTER_ID
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Campaign.resource, ID);
      response = client.get(request);
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("campaignstatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Campaignstatistics;
public class MyClass {
    /**
     * View : Statistics related to emails processed by Mailjet, grouped in a Campaign.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Campaignstatistics.resource);
      response = client.get(request);
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


