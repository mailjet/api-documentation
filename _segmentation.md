#Segmentation
##Introduction

Our contactfilter resource allows you to send newsletters to certain subsets of your contact lists. A filter can be applied to any contact metadata that you defined, like age, gender, and location.

##Prerequisites

In order to use a contact filter, you must add additional data to your contacts. Please refer to the [personalisation](#personalisation14) to see how this is done. You also have to create a contact list, because contact filters are applied to a contact list via the newsletter resource.

##How does it work?

Mailjet allows you to segment a list of contacts by creating a ContactFilter resource. This resource has a property called expression and it is the value of this property that is used to filter a list of contacts. You can create simple expressions using the <code>=</code>, <code><</code>, <code>></code> and <code>!=</code> operators:

- age>40
- gender=male
- country=France

You can also apply the negative statement <code>not</code>

But you can also combine these operators with the <code>and</code>, <code>or</code> and <code>xor</code> operator, using brackets:

- (age>40) and (gender=male)
- (country=France) and (profession=physician)
- ((age>=50) and (age<70)) or (income>50000)


Additionaly, you can filter on the contact Activities (who has opened/clicked your campaigns in the last X days). These function return a boolean.

- <code>HasOpenedSince(days)</code> 
- <code>HasClickedSince(days)</code> 

These functions are not scoped on a specific campaign. 
However, the following functions returning an integer allow you to have a sharper control in your filters:

 - <code>OpenCount(CampaignID,StartDate,EndDate)</code>
 - <code>ClickCount(URL,CampaignID,StartDate,EndDate)</code>
 - <code>UnsubCount(CampaignID,StartDate,EndDate)</code>

Parameters :

 - URL: string, exact value of the link clicked by the contact
 - CampaignID: ID of the campaign to filter on, 0 for no filtering
 - Startdate,EndDate: RFC3339 formatted date that specifies start and end date filter of the events. 0 to indicate from beginning of records and up to last record respectively.

Finally, on contact historic data, several aggregates function are available : 

 - <code>Last(name)</code>: the last value of historic field “name”
 - <code>Avg(n,name)</code>: the average of historic field “name” over the n last days. 
 - <code>Max(n,name)</code>: the maximum of historic field “name” over the n last days.
 - <code>Min(n,name)</code>: the minimum of historic field “name” over the n last days.


##Create a contact filter

```php
<?php
// Create : A filter expressions for use in newsletters.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Description" => "Only contacts aged 40",
	"Expression" => "age=40",
	"Name" => "40 year olds"
);
$result = $mj->contactfilter($params);
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
```ruby
# Create : A filter expressions for use in newsletters.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactfilter.create(
		description: "Only contacts aged 40",
		expression: "age=40",
		name: "40 year olds")
```
```python
"""
Create : A filter expressions for use in newsletters.
"""
from mailjet import Client
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
```

Lets say that we added an age property to our contacts, using the [contactmetadata](http://dev.mailjet.com/email-api/v3/contactmetadata/) and [contactdata](http://dev.mailjet.com/email-api/v3/contactdata/) resources. We now want to create a filter that gives us only those contacts that are older than 40. In order to do this, we perform a <code>POST</code> with the following properties: <code>Name</code> , <code>Expression</code> , and <code>Description</code> . Name and Description allow you to describe the filter,while <code>Expression</code> contains the actual filtering expression.

<div></div>
```php
<?php
// View : A list of filter expressions for use in newsletters.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->contactfilter($params);
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
# View : A list of filter expressions for use in newsletters.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactfilter 
```
```javascript
/**
 *
 * View : A list of filter expressions for use in newsletters.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactfilter")
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
# View : A list of filter expressions for use in newsletters.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactfilter.all()
```
```python
"""
View : A list of filter expressions for use in newsletters.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactfilter.get()
```


Now we can perform a GET request to see whether the filter has been created:


##Create a newsletter with a segmentation filter

```php
<?php
// Create : Newsletter data with segmentation.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Title" => "Mailjet greets every contact over 40",
	"Locale" => "en_US",
	"Sender" => "MisterMailjet",
	"SenderEmail" => "Mister@mailjet.com",
	"Subject" => "Greetings from Mailjet",
	"ContactsListID" => "$ID_CONTACTLIST",
	"SegmentationID" => "$ID_CONTACT_FILTER"
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
variable = Mailjet::Newsletter.create(
		title: "Mailjet greets every contact over 40",
		locale: "en_US",
		sender: "MisterMailjet",
		sender_email: "Mister@mailjet.com",
		subject: "Greetings from Mailjet",
		contacts_list_id: "$ID_CONTACTLIST",
		segmentation_id: "$ID_CONTACT_FILTER")
```
```python
"""
Create : Newsletter data with segmentation.
"""
from mailjet import Client
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
```


Segmentation is achieved by adding a contact filter to a newsletter resource.In the example below, we create a newsletter that will only be sent to contactswho are older than 40.

Where <code>your-contact-list-id</code> is the ID of the contact list you want to use and <code>your-contact-filter-id</code> is the ID of the contact filter that contains the expression <code>age>40</code> .
