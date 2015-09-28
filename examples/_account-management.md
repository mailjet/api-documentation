#Account management
##Introduction

The Mailjet REST API let’s you manage your account settings, including user information, profile details and settings for different senders.

##View Profile
```php
<?php
// View : Manage user profile data such as address, payment information etc.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->myprofile($params);
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
# View : Manage user profile data such as address, payment information etc.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/myprofile 
```
```javascript
/**
 *
 * View : Manage user profile data such as address, payment information etc.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("myprofile")
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
# View : Manage user profile data such as address, payment information etc.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Myprofile.all()
```
```python
"""
View : Manage user profile data such as address, payment information etc.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.myprofile.get()
```


View details about your profile, such as name, address and phone.


##Update Profile

```php
<?php
// Modify : Manage user profile data such as address, payment information etc.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID",
	"AddressCity" => "",
	"AddressCountry" => "",
	"AddressPostalCode" => "",
	"AddressState" => "",
	"AddressStreet" => "",
	"BillingEmail" => "",
	"BirthdayAt" => "",
	"CompanyName" => "",
	"CompanyNumOfEmployees" => "",
	"ContactPhone" => "",
	"EstimatedVolume" => "",
	"Features" => "",
	"Firstname" => "",
	"Industry" => "",
	"JobTitle" => "",
	"Lastname" => "",
	"User" => "",
	"VATNumber" => "",
	"Website" => ""
);
$result = $mj->myprofile($params);
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
# Modify : Manage user profile data such as address, payment information etc.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/myprofile/$ID \
	-H 'Content-Type: application/json' \
	-d '{
		"AddressCity":"",
		"AddressCountry":"",
		"AddressPostalCode":"",
		"AddressState":"",
		"AddressStreet":"",
		"BillingEmail":"",
		"BirthdayAt":"",
		"CompanyName":"",
		"CompanyNumOfEmployees":"",
		"ContactPhone":"",
		"EstimatedVolume":"",
		"Features":"",
		"Firstname":"",
		"Industry":"",
		"JobTitle":"",
		"Lastname":"",
		"User":"",
		"VATNumber":"",
		"Website":""
	}'
```
```javascript
/**
 *
 * Modify : Manage user profile data such as address, payment information etc.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("myprofile")
	.id($ID)
	.request({
		"AddressCity":"",
		"AddressCountry":"",
		"AddressPostalCode":"",
		"AddressState":"",
		"AddressStreet":"",
		"BillingEmail":"",
		"BirthdayAt":"",
		"CompanyName":"",
		"CompanyNumOfEmployees":"",
		"ContactPhone":"",
		"EstimatedVolume":"",
		"Features":"",
		"Firstname":"",
		"Industry":"",
		"JobTitle":"",
		"Lastname":"",
		"User":"",
		"VATNumber":"",
		"Website":""
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
# Modify : Manage user profile data such as address, payment information etc.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Myprofile.find($ID)
target.update_attributes(
		address_city: "",
		address_country: "",
		address_postal_code: "",
		address_state: "",
		address_street: "",
		billing_email: "",
		birthday_at: "",
		company_name: "",
		company_num_of_employees: "",
		contact_phone: "",
		estimated_volume: "",
		features: "",
		firstname: "",
		industry: "",
		job_title: "",
		lastname: "",
		user: "",
		vatnumber: "",
		website: "")
```
```python
"""
Modify : Manage user profile data such as address, payment information etc.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'AddressCity': '',
  'AddressCountry': '',
  'AddressPostalCode': '',
  'AddressState': '',
  'AddressStreet': '',
  'BillingEmail': '',
  'BirthdayAt': '',
  'CompanyName': '',
  'CompanyNumOfEmployees': '',
  'ContactPhone': '',
  'EstimatedVolume': '',
  'Features': '',
  'Firstname': '',
  'Industry': '',
  'JobTitle': '',
  'Lastname': '',
  'User': '',
  'VATNumber': '',
  'Website': ''
}
result = mailjet.myprofile.update(id=id, data=data)
```


Update details about your profile. For example, to update your website:

> TODO : review

```bash
curl -X PUT --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
    https://api.mailjet.com/v3/REST/myprofile \
    -H "Content-Type: application/json" \
    -d "{'Website':'www.example.com'}"
```


##View User Information

```php
<?php
// View : User account definition for Mailjet.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->user($params);
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
# View : User account definition for Mailjet.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/user 
```
```javascript
/**
 *
 * View : User account definition for Mailjet.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("user")
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
# View : User account definition for Mailjet.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::User.all()
```
```python
"""
View : User account definition for Mailjet.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.user.get()
```


With the user REST API, you can view more detailed information about the user beyond her profile, such as the last time she logged in, the associated time zone and IP address.


##Update User Information

```php
<?php
// Modify : Manage user profile data such as address, payment information etc.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID",
	"AddressCity" => "",
	"AddressCountry" => "",
	"AddressPostalCode" => "",
	"AddressState" => "",
	"AddressStreet" => "",
	"BillingEmail" => "",
	"BirthdayAt" => "",
	"CompanyName" => "",
	"CompanyNumOfEmployees" => "",
	"ContactPhone" => "",
	"EstimatedVolume" => "",
	"Features" => "",
	"Firstname" => "",
	"Industry" => "",
	"JobTitle" => "",
	"Lastname" => "",
	"User" => "",
	"VATNumber" => "",
	"Website" => ""
);
$result = $mj->myprofile($params);
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
# Modify : Manage user profile data such as address, payment information etc.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/myprofile/$ID \
	-H 'Content-Type: application/json' \
	-d '{
		"AddressCity":"",
		"AddressCountry":"",
		"AddressPostalCode":"",
		"AddressState":"",
		"AddressStreet":"",
		"BillingEmail":"",
		"BirthdayAt":"",
		"CompanyName":"",
		"CompanyNumOfEmployees":"",
		"ContactPhone":"",
		"EstimatedVolume":"",
		"Features":"",
		"Firstname":"",
		"Industry":"",
		"JobTitle":"",
		"Lastname":"",
		"User":"",
		"VATNumber":"",
		"Website":""
	}'
```
```javascript
/**
 *
 * Modify : Manage user profile data such as address, payment information etc.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("myprofile")
	.id($ID)
	.request({
		"AddressCity":"",
		"AddressCountry":"",
		"AddressPostalCode":"",
		"AddressState":"",
		"AddressStreet":"",
		"BillingEmail":"",
		"BirthdayAt":"",
		"CompanyName":"",
		"CompanyNumOfEmployees":"",
		"ContactPhone":"",
		"EstimatedVolume":"",
		"Features":"",
		"Firstname":"",
		"Industry":"",
		"JobTitle":"",
		"Lastname":"",
		"User":"",
		"VATNumber":"",
		"Website":""
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
# Modify : Manage user profile data such as address, payment information etc.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Myprofile.find($ID)
target.update_attributes(
		address_city: "",
		address_country: "",
		address_postal_code: "",
		address_state: "",
		address_street: "",
		billing_email: "",
		birthday_at: "",
		company_name: "",
		company_num_of_employees: "",
		contact_phone: "",
		estimated_volume: "",
		features: "",
		firstname: "",
		industry: "",
		job_title: "",
		lastname: "",
		user: "",
		vatnumber: "",
		website: "")
```
```python
"""
Modify : Manage user profile data such as address, payment information etc.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'AddressCity': '',
  'AddressCountry': '',
  'AddressPostalCode': '',
  'AddressState': '',
  'AddressStreet': '',
  'BillingEmail': '',
  'BirthdayAt': '',
  'CompanyName': '',
  'CompanyNumOfEmployees': '',
  'ContactPhone': '',
  'EstimatedVolume': '',
  'Features': '',
  'Firstname': '',
  'Industry': '',
  'JobTitle': '',
  'Lastname': '',
  'User': '',
  'VATNumber': '',
  'Website': ''
}
result = mailjet.myprofile.update(id=id, data=data)
```


Update user information. For example, to update your last name you could do the following:


> TODO : review

```bash
curl -X PUT --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
    https://api.mailjet.com/v3/REST/myprofile \
    -H "Content-Type: application/json" \
    -d "{'LastName':'John'}"
```


##Create Sender Information

```php
<?php
// Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Email" => "anothersender@mailjet.com"
);
$result = $mj->sender($params);
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
# Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/sender \
	-H 'Content-Type: application/json' \
	-d '{
		"Email":"anothersender@mailjet.com"
	}'
```
```javascript
/**
 *
 * Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("sender")
	.request({
		"Email":"anothersender@mailjet.com"
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
# Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Sender.create(
		email: "anothersender@mailjet.com")
```
```python
"""
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Email': 'anothersender@mailjet.com'
}
result = mailjet.sender.create(data=data)
```


To create a sender, provide the email address of the sender as part of a POST.

##View Sender Information

To view information related to a sender, use the REST API on the sender resource!Note: Sender email addresses must be confirmed before they can be used to send messages.

##Update Sender Email

```php
<?php
// Modify : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "PUT",
	"ID" => "$ID",
	"EmailType" => "",
	"IsDefaultSender" => "false",
	"Name" => ""
);
$result = $mj->sender($params);
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
# Modify : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
curl -s \
	-X PUT \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/sender/$ID \
	-H 'Content-Type: application/json' \
	-d '{
		"EmailType":"",
		"IsDefaultSender":"false",
		"Name":""
	}'
```
```javascript
/**
 *
 * Modify : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.put("sender")
	.id($ID)
	.request({
		"EmailType":"",
		"IsDefaultSender":"false",
		"Name":""
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
# Modify : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Sender.find($ID)
target.update_attributes(
		email_type: "",
		is_default_sender: "false",
		name: "")
```
```python
"""
Modify : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
data = {
  'EmailType': '',
  'IsDefaultSender': 'false',
  'Name': ''
}
result = mailjet.sender.update(id=id, data=data)
```


To update sender information, use either the Sender ID or email address (as this is an unique field) and include it as part of a PUT request.

> TODO : review

```bash
curl -X PUT --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
    https://api.mailjet.com/v3/REST/sender/$SenderID \
    -H "Content-Type: application/json" \
    -d "{'Name':'Another Sender'}"
```


##Delete Sender Information

```php
<?php
// Delete : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "DELETE",
	"ID" => "$ID",
);
$result = $mj->sender($params);
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
# Delete : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
curl -s \
	-X DELETE \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/sender/$ID \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Delete : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.delete("sender")
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
# Delete : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
target = Mailjet::Sender.find($ID)
target.delete()
```
```python
"""
Delete : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.sender.delete(id=id)
```


To delete sender information, use either the sender ID or email address and include it as part of a DELETE request. Note: There will be no confirmation upon a successful delete. You will need to perform another API call to verify the sender information has been properly removed.


