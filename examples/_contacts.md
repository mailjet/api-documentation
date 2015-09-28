# Contacts

## Introduction

Before you can send any emails, you must first create contacts to receive those emails. Contacts in the Mailjet system have a few basic properties, such as a name and an email address. If you need to store more information about your contact, you can also add custom properties using [metadata](#contactmetadata).

The property reference guide for this resource can be found [HERE](http://dev.mailjet.com/email-api/v3/contact/)

## Creating contacts

```php
<?php
// Create : Manage the details of a Contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Email" => "Mister@mailjet.com"
);
$result = $mj->contact($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
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
variable = Mailjet::Contact.create(
		email: "Mister@mailjet.com")
```
```python
"""
Create : Manage the details of a Contact.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Email': 'Mister@mailjet.com'
}
result = mailjet.contact.create(data=data)
```
 

> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "",
			"DeliveredCount": "",
			"Email": "Mister@mailjet.com",
			"ID": "",
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


To create a new contact, perform a POST with any relevant contact data. An email address is the only required field.

Using the example, a new contact with an email address of Mister@mailjet.com will be created. If successful, you will receive confirmation output with details about the new contact object.

If you want to add custom data for a contact (for example age or country), you can create the property using the <code>/contactmetadata</code> method, information on which can be found [HERE](#contactmetadata).

NB: Contactmetadata properties are available to any Contact you have in the system as a whole and can be used in any Contact List.

## Viewing contacts

```php
<?php
// View : Manage the details of a Contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : Manage the details of a Contact.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact 
```
```javascript
/**
 *
 * View : Manage the details of a Contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
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
# View : Manage the details of a Contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all()
```
```python
"""
View : Manage the details of a Contact.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contact.get()
```
 

> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "",
			"DeliveredCount": "",
			"Email": "Mister@mailjet.com",
			"ID": "",
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


To list all of the contacts in the system, perform a <code>GET</code> request.

<div></div>

```php
<?php
// View : Manage the details of a single Contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "VIEW",
	"ID" => "$ID_OR_EMAIL",
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : Manage the details of a single Contact.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$ID_OR_EMAIL 
```
```javascript
/**
 *
 * View : Manage the details of a single Contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.id($ID_OR_EMAIL)
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
# View : Manage the details of a single Contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.find($ID_OR_EMAIL)
```
```python
"""
View : Manage the details of a single Contact.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_OR_EMAIL'
result = mailjet.contact.get(id=id)
```
 


Use the ID or email address of a specific contact to return only that contact’s information. 


## Updating contacts

```php
<?php
// this is call to update a contact
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID_OR_EMAIL",
	"name" => "john"
);
$result = $mj->contact($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# this is call to update a contact
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$ID_OR_EMAIL \
	-H 'Content-Type: application/json' \
	-d '{
		"name":"john"
	}'
```
```javascript
/**
 *
 * this is call to update a contact
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("contact")
	.id($ID_OR_EMAIL)
	.request({
		"name":"john"
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
# this is call to update a contact
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Contact.find($ID_OR_EMAIL)
target.update_attributes(
		name: "john")
```
```python
"""
this is call to update a contact
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_OR_EMAIL'
data = {
  'name': 'john'
}
result = mailjet.contact.update(id=id, data=data)
```
 

> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"CreatedAt": "",
			"DeliveredCount": "",
			"Email": "Mister@mailjet.com",
			"ID": "",
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


If you need to update any contact information, perform a <code>PUT</code> with the properties you wish to change. Let’s edit the contact by updating his name to John.

The object will only be returned when contact information is modified. If there is no change to that contact’s information, the API will return a <code>HTTP 304</code> status code, which specifies 'Content not changed'.

<h2 id="contactmetadata">Personalisation</h2>

The reference guide for this resource can be found [HERE](http://dev.mailjet.com/email-api/v3/contactmetadata/)

The contact resource allows you to create contacts using their email addresses and names. If you want to add more granular details about your contacts, Mailjet provides that capability by allowing you to add custom data to contacts. To do so, we have to define what extra information we wish to store with our contacts (perhaps what country the contact lives in or how old the contact is) and how that data will be stored (for example, will it be stored as a string or a number). This is contact 'metadata'.



The contact resource allows you to create contacts using their email addresses and names. What if we wanted to add more granular details about our contacts? Mailjet provides that capability by allowing you to add custom data to contacts. To do so, we will first need to define the metadata for our contacts. In other words, we have to define exactly what extra information we wish to store with our contacts (perhaps what country the contact lives in or how old the contact is) and how that data will be stored (for example, will it be stored as a string or a number).  Before you can send any emails, you must first create contacts to receive those emails. Contacts in the Mailjet system have a few basic properties, such as a name and an email address. If you need to store more information about your contact, you can also add custom properties using metadata.


### Defining custom Contact data

```php
<?php
// Create : Definition of available extra data items for contacts.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Datatype" => "str",
	"Name" => "Age",
	"NameSpace" => "static"
);
$result = $mj->contactmetadata($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
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
variable = Mailjet::Contactmetadata.create(
		datatype: "str",
		name: "Age",
		name_space: "static")
```
```python
"""
Create : Definition of available extra data items for contacts.
"""
from mailjet import Client
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
```


> Response

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


To define custom contact data, perform a POST with the following properties:

 - Name - the name of the custom data field
 - DataType - the type of data that is being stored (this can be either a str, int, float, or bool)
 - Namespace - this can be either <code>static</code> or <code>historic</code>

For example, if we wanted to store the age of each of our contacts, we could add an "Age" property

<div></div>
### Adding custom static Contact data

> TODO: json array format broken

```php
<?php
// Modify : Modify the static custom contact data
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$CONTACT_ID",
	"Data" => json_decode('[
				{
						"Name": "Age",
						"value": 30
				},
				{
						"Name": "Country",
						"value": "US"
				}
		]', true)
);
$result = $mj->contactdata($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
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
target.update_attributes(
		data: [{ 'Name'=> 'Age', 'value'=> 30}, { 'Name'=> 'Country', 'value'=> 'US'}])
```
```python
"""
Modify : Modify the static custom contact data
"""
from mailjet import Client
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
```


Once we have successfully defined the custom data that our contacts can store, we can then store those values with our contacts as new pieces of data. Continuing our example above, let's set John Smith's age to be 30 by performing a PUT with the new key value pairs as our data.

<div></div>
### Adding custom historic Contact data

```php
<?php
// Create : This resource can be used to add historical data to contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"ContactID" => "$CONTACT_ID",
	"Data" => "10",
	"Name" => "Purchase"
);
$result = $mj->contacthistorydata($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
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
variable = Mailjet::Contacthistorydata.create(
		contact_id: "$CONTACT_ID",
		data: "10",
		name: "Purchase")
```
```python
"""
Create : This resource can be used to add historical data to contact.
"""
from mailjet import Client
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
```


Once we have successfully defined the custom data that our contacts can store, we can then store those values with our contacts as new pieces of data. Continuing our example above, let's set John Smith's age to be 30 by performing a PUT with the new key value pairs as our data.

<div></div>
### Using custom Contact data in Mailjet

These data can be used to personalise [newsletters]() and [transactional messages](). Contact's properties can also be used to [segment]() contact lists.

The syntax to use contact's data in messages is <code>[[data:metadata_name]]</code> where metadata_name is the name given to the already defined metadata.
By default, the placeholder will be replaced by an empty string if the metadata is empty for the current contact. A default value can also be specified by adding it at the end of the placeholder like this: <code>[[data:name:"John Doe"]]</code>. 

<div></div>
### Viewing Custom Contact Data definitions

```php
<?php
// View : Definition of available extra data items for contacts.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contactmetadata($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : Definition of available extra data items for contacts.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactmetadata 
```
```javascript
/**
 *
 * View : Definition of available extra data items for contacts.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactmetadata")
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
# View : Definition of available extra data items for contacts.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactmetadata.all()
```
```python
"""
View : Definition of available extra data items for contacts.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactmetadata.get()
```

> Response

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


<div></div>
### Viewing Custom static Contact Data 

```php
<?php
// View : This resource can be used to examine and manipulate the associated extra static data of a contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contactdata($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : This resource can be used to examine and manipulate the associated extra static data of a contact.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactdata 
```
```javascript
/**
 *
 * View : This resource can be used to examine and manipulate the associated extra static data of a contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactdata")
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
# View : This resource can be used to examine and manipulate the associated extra static data of a contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactdata.all()
```
```python
"""
View : This resource can be used to examine and manipulate the associated extra static data of a contact.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactdata.get()
```



<div></div>
### Viewing Custom historic Contact Data 

```php
<?php
// View : This resource can be used to examine the associated extra historical data of a contact.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contacthistorydata($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : This resource can be used to examine the associated extra historical data of a contact.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contacthistorydata 
```
```javascript
/**
 *
 * View : This resource can be used to examine the associated extra historical data of a contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contacthistorydata")
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
# View : This resource can be used to examine the associated extra historical data of a contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contacthistorydata.all()
```
```python
"""
View : This resource can be used to examine the associated extra historical data of a contact.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contacthistorydata.get()
```





## Getting the lists for a given Contact

```php
<?php
// View : Lists a contact belong to
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "VIEW",
	"ID" => "$ID",
);
$result = $mj->contactGetContactsLists($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : Lists a contact belong to
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$ID/getcontactslists 
```
```javascript
/**
 *
 * View : Lists a contact belong to
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.id($ID)
	.action("getcontactslists")
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
# View : Lists a contact belong to
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact_getcontactslists.find($ID)
```
```python
"""
View : Lists a contact belong to
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.contact_getcontactslists.get(id=id)
```


> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"IsActive": "true",
			"IsUnsub": "false",
			"ListID": "1"
		}
	],
	"Total": 1
}
```


To view the Lists that include a specified Contact, you can do a <code>GET</code> request to <code>contact/$ID/getcontactslists</code>:


## Deleting contacts

  At the moment there is no way to permanently delete contacts from the system. You can, however, add and remove contacts from custom lists.
