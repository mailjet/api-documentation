#Contacts list
##Introduction

The reference guide for this resource can be found [HERE](http://dev.mailjet.com/email-api/v3/contactslist/)

To better organize your contacts and how you target them with emails, you can group your contacts into lists. Note: Setting up a contact list is a prerequisite to creating a campaign.

##Creating Contact Lists

```php
<?php
// Create : only need a Name
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Name" => "myList"
);
$result = $mj->contactslist($params);
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
variable = Mailjet::Contactslist.create(
		name: "myList")
```
```python
"""
Create : only need a Name
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Name': 'myList'
}
result = mailjet.contactslist.create(data=data)
```


> Response

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


To create a new contact list, perform a <code>POST</code> with the name of the contact list.

##Viewing Contact Lists

```php
<?php
// View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contactslist($params);
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
# View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist 
```
```javascript
/**
 *
 * View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactslist")
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
# View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist.all()
```
```python
"""
View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactslist.get()
```


> Response

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


Similar to viewing an individual contact or all of your contacts, you can also view a single contact list or all of your contact lists.


##Updating Contact Lists

```php
<?php
// Modify : Manage your contact lists. One Contact might be associated to one or more ContactsList.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID",
	"IsDeleted" => "false",
	"Name" => "myList"
);
$result = $mj->contactslist($params);
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
# Modify : Manage your contact lists. One Contact might be associated to one or more ContactsList.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist/$ID \
	-H 'Content-Type: application/json' \
	-d '{
		"IsDeleted":"false",
		"Name":"myList"
	}'
```
```javascript
/**
 *
 * Modify : Manage your contact lists. One Contact might be associated to one or more ContactsList.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("contactslist")
	.id($ID)
	.request({
		"IsDeleted":"false",
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
# Modify : Manage your contact lists. One Contact might be associated to one or more ContactsList.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Contactslist.find($ID)
target.update_attributes(
		is_deleted: "false",
		name: "myList")
```
```python
"""
Modify : Manage your contact lists. One Contact might be associated to one or more ContactsList.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'IsDeleted': 'false',
  'Name': 'myList'
}
result = mailjet.contactslist.update(id=id, data=data)
```


If you want to update a property of the Contact List (in this example the Name), this can be acheived via a <code>PUT</code>:

> Response

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



##Managing a Contact subscription to a List

> TODO: json array format broken

```php
<?php
// Add a contact to the list
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"ID" => "$LIST_ID",
	"Email" => "mrsmith@mailjet.com",
	"Name" => "MrSmith",
	"Action" => "addnoforce",
	"Properties" => json_decode('{
				"property1": "value",
				"propertyN": "valueN"
		}', true)
);
$result = $mj->contactslistManageContact($params);
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
```ruby
# Add a contact to the list
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist_managecontact.create(id: $LIST_ID,
		email: "mrsmith@mailjet.com",
		name: "MrSmith",
		action: "addnoforce",
		properties: { 'property1'=> 'value', 'propertyN'=> 'valueN'})
```
```python
"""
Add a contact to the list
"""
from mailjet import Client
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
```
 

        
After creating a contact list, you’ll want to add contacts to that list. We do this by performing a <code>POST</code> using the <code>contactslist/$ID/managecontact</code> REST method and passing in contact details.

##Removing a Contact from a List

```php
<?php
// Create : only need a Name
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Name" => "myList"
);
$result = $mj->contactslist($params);
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
variable = Mailjet::Contactslist.create(
		name: "myList")
```
```python
"""
Create : only need a Name
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Name': 'myList'
}
result = mailjet.contactslist.create(data=data)
```


Removing a contact from a list shares the same syntax as adding one to a list, with the Action 'remove' specified in the body of the <code>POST</code> request

## Getting the Lists for a given Contact

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


To view the Lists that include a specified Contact, you can do a <code>GET</code> request to <code>contact/$ID/getcontactslists</code>

## Viewing Contacts on a List

```php
<?php
// View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contactslist($params);
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
# View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactslist 
```
```javascript
/**
 *
 * View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactslist")
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
# View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactslist.all()
```
```python
"""
View : Manage your contact lists. One Contact might be associated to one or more ContactsList.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactslist.get()
```


To view contacts on a Contact List, we can add a filter to the Contact action and specify the ID of the list we want to view. The default API call will return results and we can increase or decrease this using the limit filter. If you want to show ALL contacts on a list, set the limit to -1


