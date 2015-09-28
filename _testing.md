# ===REMOVED TO PROCESS LATER==

# Shubham - Test

## A

This is why 
```http://api.mailjet.com

### AA1

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


Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse

Table Header 1 | Table Header 2 | Table Header 3
-------------- | -------------- | --------------
Row 1 col 1 | Row 1 col 2 | Row 1 col 3
Row 2 col 1 | Row 2 col 2 | Row 2 col 3

cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.


### AA2


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


Then you need to create some contact resources which will be your recipients. To do so, just specify an email address:

### AA3

1. This
2. Is
3. An
4. Ordered
5. List


Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

```php
<?php
// Delete : Manage the relationship between a contact and a contactslists.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "DELETE",
	"ID" => "$ID",
);
$result = $mj->listrecipient($params);
if ($mj->_response_code == 204){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# Delete : Manage the relationship between a contact and a contactslists.
curl -s \
	-X DELETE \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/listrecipient/$ID \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Delete : Manage the relationship between a contact and a contactslists.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.delete("listrecipient")
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
```ruby
# Delete : Manage the relationship between a contact and a contactslists.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Listrecipient.find($ID)
target.delete()
```
```python
"""
Delete : Manage the relationship between a contact and a contactslists.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.listrecipient.delete(id=id)
```


Last step: you have to link this contact to your contactslist previously created, so you need to create a new listrecipient.

Don't forget the IsActive field which is false by default and would not allow you to send emails to these recipients.
Now you can begin sending newsletters to your contacts list created. Newsletters represent the core resource of the Mailjet API. Along with customizing the recipients of newsletters, you can also customize the look and feel of the newsletters themselves.
In all the examples below, you need to replace $ID with the id of your newsletter. This is an [internal link](#error-code-definitions), this is an [external link](http://google.com).

## B

To create a newsletter, perform a POST with relevant newsletter data. Required fields are a Locale, Sender, SenderEmail, Subject and ContactsListID.

<aside class="success">
You must replace `coucou` with your personal API key.
</aside>

<aside class="warning">
You must replace `coucou` with your personal Bird.
</aside>

### BB

```php
<?php
// View : Manage event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->eventcallbackurl($params);
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
# View : Manage event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/eventcallbackurl 
```
```javascript
/**
 *
 * View : Manage event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("eventcallbackurl")
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
# View : Manage event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Eventcallbackurl.all()
```
```python
"""
View : Manage event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.eventcallbackurl.get()
```


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"ContactID": "",
			"ID": "",
			"IsActive": "false",
			"IsUnsubscribed": "false",
			"ListID": "",
			"UnsubscribedAt": ""
		}
	],
	"Total": 1
}
```


Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

<aside class="notice">
You must replace `coucou` with your personal API key.
</aside>

![/images/apikeys-screen.png](/images/apikeys-screen.png)

This specification page was created with [Slate](http://github.com/tripit/slate). Feel free to edit it on [https://github.com/mailjet/template-builder-technical-docs](https://github.com/mailjet/template-builder-technical-docs)

##From Getting Started



###List all contacts:

<div></div>

```bash
curl --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" https://api.mailjet.com/v3/REST
/contact/$id
```

###Show a single contact using the contact id:

<div></div>

```bash
curl --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" https://api.mailjet.com/v3/REST
/contact/[encoded-email-address]
```

###Show a single contact using the contact email:
<div></div>

```bash
curl -X PUT --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" https://api.mailjet.com/v3
/REST/contact/:id -H "Content-Type: application/json" -d '{"Name":"MJ Developer"}'
```

TIP: Often you already have an email address and you just want to see whether it already exists or if the contact data for this email is correct. You can then use the email as a unique identifier instead of the contact id. This way you don't have to retrieve and iterate through a contact list.



###Update a contact:

<div></div>

```bash
curl -s -X POST --user "$APIKEY:$APISECRET" https://api.mailjet.com/v3/REST/contactmetadata {"Name":"Age","DataType":"int"}
```

```bash
curl -s -X PUT --user "$APIKEY:$APISECRET" \
    https://api.mailjet.com/v3/REST/contactdata/1 \
    -H "Content-Type: application/json" \
    -d '{"Data" : [{"Name" : "Age", "value": 42}] }'
```

##Send API 

> TODO : review code generation 

> TODO : recipient is not an object and is probably going to be deprecated 

```php
<?php
// This calls sends an email to the given recipient.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"FromEmail" => "pilot@mailjet.com",
	"FromName" => "Mailjet's Pilot",
	"Sender" => "pilot@mailjet.com",
	"Recipient" => json_decode('{
				"Email": "passenger@mailjet.com",
				"Name": "Passenger",
				"Vars": {
						"name": "Yann Solo",
						"var2": "String value 2",
						"varN": "String value N"
				}
		}', true),
	"Subject" => "Your email flight plan!",
	"Text-part" => "Dear passenger [[var:name]], welcome to Mailjet! May the delivery force be with you! Today: [[var:perk]]!",
	"Html-part" => "<html>Dear passenger [[var:name]], welcome to Mailjet!</br>May the delivery force be with you! Today: [[var:perk]]!</html>",
	"Mj-prio" => "2",
	"Mj-campaign" => "Email_Flight-Plan 1",
	"Mj-deduplicatecampaign" => "true OR y OR 1 OR NOTHING",
	"Mj-trackopen" => "0 OR 1",
	"Mj-trackclick" => "0 OR 1",
	"Mj-CustomID" => "Trackable_email_1",
	"Mj-EventPayload" => "",
	"Headers" => "",
	"Vars" => json_decode('{
				"perk": "free Millenium Falcon",
				"globalVar": "String value"
		}', true),
	"Recipients" => json_decode('[
				{
						"Email": "y.solo@mailjet.com",
						"Name": "Passenger",
						"Vars": {
								"name": "Yann Solo",
								"var2": "String value 2",
								"varN": "String value N"
						}
				},
				{
						"Email": "luke@mailjet.com",
						"Name": "Luke",
						"Vars": {
								"name": "Placeholder1",
								"var2": "String value 2",
								"varN": "String value N"
						}
				}
		]', true),
	"Attachments" => "array("1stAttachment" => "@path/to/attachment.*", "@path/to/attachment2.*")",
	"Inline_attachments" => "array("1stInlineAttachment" => "@path/to/inlineAttachment.*", "@path/to/inlineAttachment2.*")"
);
$result = $mj->sendMessage($params);
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
# This calls sends an email to the given recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/send/message \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet's Pilot",
		"Sender":"pilot@mailjet.com",
		"Recipient":{
				"Email": "passenger@mailjet.com",
				"Name": "Passenger",
				"Vars": {
						"name": "Yann Solo",
						"var2": "String value 2",
						"varN": "String value N"
				}
		},
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger [[var:name]], welcome to Mailjet! May the delivery force be with you! Today: [[var:perk]]!",
		"Html-part":"<html>Dear passenger [[var:name]], welcome to Mailjet!</br>May the delivery force be with you! Today: [[var:perk]]!</html>",
		"Mj-prio":"2",
		"Mj-campaign":"Email_Flight-Plan 1",
		"Mj-deduplicatecampaign":"true OR y OR 1 OR NOTHING",
		"Mj-trackopen":"0 OR 1",
		"Mj-trackclick":"0 OR 1",
		"Mj-CustomID":"Trackable_email_1",
		"Mj-EventPayload":"",
		"Headers":"",
		"Vars":{
				"perk": "free Millenium Falcon",
				"globalVar": "String value"
		},
		"Recipients":[
				{
						"Email": "y.solo@mailjet.com",
						"Name": "Passenger",
						"Vars": {
								"name": "Yann Solo",
								"var2": "String value 2",
								"varN": "String value N"
						}
				},
				{
						"Email": "luke@mailjet.com",
						"Name": "Luke",
						"Vars": {
								"name": "Placeholder1",
								"var2": "String value 2",
								"varN": "String value N"
						}
				}
		],
		"Attachments":"array("1stAttachment" => "@path/to/attachment.*", "@path/to/attachment2.*")",
		"Inline_attachments":"array("1stInlineAttachment" => "@path/to/inlineAttachment.*", "@path/to/inlineAttachment2.*")"
	}'
```
```javascript
/**
 *
 * This calls sends an email to the given recipient.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("send")
	.action("message")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet's Pilot",
		"Sender":"pilot@mailjet.com",
		"Recipient":{
				"Email": "passenger@mailjet.com",
				"Name": "Passenger",
				"Vars": {
						"name": "Yann Solo",
						"var2": "String value 2",
						"varN": "String value N"
				}
		},
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger [[var:name]], welcome to Mailjet! May the delivery force be with you! Today: [[var:perk]]!",
		"Html-part":"<html>Dear passenger [[var:name]], welcome to Mailjet!</br>May the delivery force be with you! Today: [[var:perk]]!</html>",
		"Mj-prio":"2",
		"Mj-campaign":"Email_Flight-Plan 1",
		"Mj-deduplicatecampaign":"true OR y OR 1 OR NOTHING",
		"Mj-trackopen":"0 OR 1",
		"Mj-trackclick":"0 OR 1",
		"Mj-CustomID":"Trackable_email_1",
		"Mj-EventPayload":"",
		"Headers":"",
		"Vars":{
				"perk": "free Millenium Falcon",
				"globalVar": "String value"
		},
		"Recipients":[
				{
						"Email": "y.solo@mailjet.com",
						"Name": "Passenger",
						"Vars": {
								"name": "Yann Solo",
								"var2": "String value 2",
								"varN": "String value N"
						}
				},
				{
						"Email": "luke@mailjet.com",
						"Name": "Luke",
						"Vars": {
								"name": "Placeholder1",
								"var2": "String value 2",
								"varN": "String value N"
						}
				}
		],
		"Attachments":"array("1stAttachment" => "@path/to/attachment.*", "@path/to/attachment2.*")",
		"Inline_attachments":"array("1stInlineAttachment" => "@path/to/inlineAttachment.*", "@path/to/inlineAttachment2.*")"
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
# This calls sends an email to the given recipient.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send_message.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet's Pilot",
		sender: "pilot@mailjet.com",
		recipient: { 'Email'=> 'passenger@mailjet.com', 'Name'=> 'Passenger', 'Vars'=> { 'name'=> 'Yann Solo', 'var2'=> 'String value 2', 'varN'=> 'String value N' }},
		subject: "Your email flight plan!",
		text_part: "Dear passenger [[var:name]], welcome to Mailjet! May the delivery force be with you! Today: [[var:perk]]!",
		html_part: "<html>Dear passenger [[var:name]], welcome to Mailjet!</br>May the delivery force be with you! Today: [[var:perk]]!</html>",
		mj_prio: "2",
		mj_campaign: "Email_Flight-Plan 1",
		mj_deduplicatecampaign: "true OR y OR 1 OR NOTHING",
		mj_trackopen: "0 OR 1",
		mj_trackclick: "0 OR 1",
		mj_custom_id: "Trackable_email_1",
		mj_event_payload: "",
		headers: "",
		vars: { 'perk'=> 'free Millenium Falcon', 'globalVar'=> 'String value'},
		recipients: [{ 'Email'=> 'y.solo@mailjet.com', 'Name'=> 'Passenger', 'Vars'=> { 'name'=> 'Yann Solo', 'var2'=> 'String value 2', 'varN'=> 'String value N' }}, { 'Email'=> 'luke@mailjet.com', 'Name'=> 'Luke', 'Vars'=> { 'name'=> 'Placeholder1', 'var2'=> 'String value 2', 'varN'=> 'String value N' }}],
		attachments: "array("1stAttachment" => "@path/to/attachment.*", "@path/to/attachment2.*")",
		inline_attachments: "array("1stInlineAttachment" => "@path/to/inlineAttachment.*", "@path/to/inlineAttachment2.*")")
```
```python
"""
This calls sends an email to the given recipient.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet's Pilot',
  'Sender': 'pilot@mailjet.com',
  'Recipient': {
				"Email": "passenger@mailjet.com",
				"Name": "Passenger",
				"Vars": {
						"name": "Yann Solo",
						"var2": "String value 2",
						"varN": "String value N"
				}
		},
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger [[var:name]], welcome to Mailjet! May the delivery force be with you! Today: [[var:perk]]!',
  'Html-part': '<html>Dear passenger [[var:name]], welcome to Mailjet!</br>May the delivery force be with you! Today: [[var:perk]]!</html>',
  'Mj-prio': '2',
  'Mj-campaign': 'Email_Flight-Plan 1',
  'Mj-deduplicatecampaign': 'true OR y OR 1 OR NOTHING',
  'Mj-trackopen': '0 OR 1',
  'Mj-trackclick': '0 OR 1',
  'Mj-CustomID': 'Trackable_email_1',
  'Mj-EventPayload': '',
  'Headers': '',
  'Vars': {
				"perk": "free Millenium Falcon",
				"globalVar": "String value"
		},
  'Recipients': [
				{
						"Email": "y.solo@mailjet.com",
						"Name": "Passenger",
						"Vars": {
								"name": "Yann Solo",
								"var2": "String value 2",
								"varN": "String value N"
						}
				},
				{
						"Email": "luke@mailjet.com",
						"Name": "Luke",
						"Vars": {
								"name": "Placeholder1",
								"var2": "String value 2",
								"varN": "String value N"
						}
				}
		],
  'Attachments': 'array("1stAttachment" => "@path/to/attachment.*", "@path/to/attachment2.*")',
  'Inline_attachments': 'array("1stInlineAttachment" => "@path/to/inlineAttachment.*", "@path/to/inlineAttachment2.*")'
}
result = mailjet.send_message.create(data=data)
```


<aside class="warning">
TO REMOVE LATER : formdata field names  
</aside>

Property Name | Description 
  ----------|------------
from | Must be a valid, activated and registered sender for this account <br />May include the name part: <code>john@example.com</code> or <code><john@example.com></code> or <code>"John Doe" <john@example.com></code><br />**MANDATORY - MAX FROM: 1**
sender | This can be set only on given API Keys. Contact the [support team](https://app.mailjet.com/support/ticket) if you want us to enable this setting on your account.<br />Must be a valid active sender for this account.<br />Perform a simple GET on resource <code>/sender</code> to view a list of allowed senders for your account, or within the Mailjet Account Settings under Sender Addresses<br />**MAX SENDER: 1**
to | May include the name part: <code>john@example.com</code> or <code><john@example.com></code> or <code>"John Doe" <john@example.com></code><br />If a recipient is specified twice (in the to, cc, or bcc), it is counted only once.<br />Can be a magic list @lists.mailjet.com. See the <code>Address</code> [contactslist](/email-api/v3/contactslist/) property.<br />**MANDATORY - MAX RECIPIENTS: 50**
cc, bcc | May include the name part: <code>john@example.com</code> or <code><john@example.com></code> or <code>"John Doe" <john@example.com></code><br />If one recipient is specified twice, count as one only (including to, cc, bcc)<br />**MAX RECIPIENTS: 50**
subject | At least 1 char, maximum length is 255 chars <br />**MANDATORY - MAX SUBJECTS: 1**
text | Provides the Text part of the message<br />Mandatory if the HTML param is not specified<br />**MANDATORY IF NO HTML - MAX PARTS: 1**
html | Provides the HTML part of the message<br />Mandatory if the text param is not specified<br />**MANDATORY IF NO TEXT - MAX PARTS: 1**
attachment | Attach files automatically to this Email<br />Sum of all attachments, including inline may not exceed 15 MB total
inlineattachment | Attach a file for inline use via <code>cid:basename_of_file.ext</code><br />Sum of all attachements, including inline may not exceed 15 MB total
mj-prio | Manage message processing priority inside your account (API key) scheduling queue.<br />Default is <code>2</code> as in the SMTP submission.<br />Equivalent of using <code>X-Mailjet-Prio</code> header through SMTP<br />[More information](https://app.mailjet.com/docs/email-priority-management)
mj-campaign | Groups multiple messages in one campaign<br />Equivalent of using <code>X-Mailjet-Campaign</code> header through SMTP.<br />[More information](https://app.mailjet.com/docs/emails_headers)
mj-deduplicatecampaign | Deduplicate duplicates inside one campaign.<br />Equivalent of using <code>X-Mailjet-DeduplicateCampaign</code> header through SMTP.<br />Can only be used if mj-campaign is specified.<br />[More information](https://app.mailjet.com/docs/emails_headers)
mj-trackopen | Force or disable open tracking on this message, overriding preferences.<br />Equivalent of using <code>X-Mailjet-TrackOpen</code> header through SMTP.<br />Can only be used with a HTML part.<br />[More information](https://app.mailjet.com/docs/emails_headers)
mj-trackclick | Force or disable click tracking on this message, overriding preferences.<br />Equivalent to using the <code>X-Mailjet-TrackClick</code> header through SMTP.<br />Can only be specified if the HTML part is provided<br />[More information](https://app.mailjet.com/docs/emails_headers)
mj-customid | Attach a custom ID to the message<br />Equivalent to using the X-MJ-CustomID header through SMTP.
mj-eventpayload | Attach a payload to the message<br />Equivalent to using the X-MJ-EventPayload header through SMTP.
header | Add a line to the email's headers<br />This field must not contain a carriage-return and must comply with RFC format.<br />Sample: <code>X-My-Header: my own value</code>

## Other newsletter resources

### View newsletters

```php
<?php
// View : Newsletter data.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->newsletter($params);
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
# View : Newsletter data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter 
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
# View : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter.all()
```
```python
"""
View : Newsletter data.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.newsletter.get()
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
			"CampaignID": "",
			"ContactsListID": "",
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
			"SegmentationID": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"TemplateID": "",
			"TestAddress": "",
			"Title": "",
			"Url": ""
		}
	],
	"Total": 1
}
```


To list all of the newsletters in the system, perform a GET request.

<div></div>

```php
<?php
// View : Newsletter data.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "VIEW",
	"ID" => "$ID",
);
$result = $mj->newsletter($params);
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
# View : Newsletter data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID 
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
```ruby
# View : Newsletter data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Newsletter.find($ID)
```
```python
"""
View : Newsletter data.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.newsletter.get(id=id)
```


You can also view a single newsletter by passing the ID of the newsletter in the request:


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
			"CampaignID": "",
			"ContactsListID": "",
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
			"SegmentationID": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"TemplateID": "",
			"TestAddress": "",
			"Title": "",
			"Url": ""
		}
	],
	"Total": 1
}
```


<div></div>
### Update newsletters

```php
<?php
// Modify : Newsletter data.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID",
	"Title" => "Updated Newsletter Title"
);
$result = $mj->newsletter($params);
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
# Modify : Newsletter data.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/newsletter/$ID \
	-H 'Content-Type: application/json' \
	-d '{
		"Title":"Updated Newsletter Title"
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
	.request({
		"Title":"Updated Newsletter Title"
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
target = Mailjet::Newsletter.find($ID)
target.update_attributes(
		title: "Updated Newsletter Title")
```
```python
"""
Modify : Newsletter data.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'Title': 'Updated Newsletter Title'
}
result = mailjet.newsletter.update(id=id, data=data)
```


> The response

```json
{
	"Count": 1,
	"Data": [
		{
			"AXFraction": "",
			"AXFractionName": "",
			"AXTesting": "",
			"Callback": "",
			"CampaignID": "",
			"ContactsListID": "",
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
			"SegmentationID": "",
			"Sender": "MisterMailjet",
			"SenderEmail": "Mister@mailjet.com",
			"SenderName": "",
			"Status": "",
			"Subject": "Greetings from Mailjet",
			"TemplateID": "",
			"TestAddress": "",
			"Title": "",
			"Url": ""
		}
	],
	"Total": 1
}
```


Newsletters have a number of properties that can all be updated programmatically with a PUT request.

###Newsletter status


```bash
curl -X GET \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v3/REST/newsletter/$ID/status
```

>API response:

```json
{
   Count: 1,
   Data: [
      {
         Status: "draft"
      }
   ],
   Total: 1
}
```

At all time, you can retrieve the status of a given newsletter by performing a <code>GET</code> request on the newsletter action <code>status</code>. Here are the different values that you can have:

 - deleted
 - archived draft
 - draft
 - programmed
 - sent


<div></div>


###Delete Newsletters
At the moment there is no way to permanently delete newsletters from the system.


