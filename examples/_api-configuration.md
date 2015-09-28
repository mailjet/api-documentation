#API Configuration
##Introduction

Your API key and secret key are the username and password used to access all of Mailjet's APIs.  Mailjet provides a particular endpoint for managing all aspects of these keys - creating more API keys for subaccounts, setting master account and subaccount, viewing quarantine levels, and activating and deactivating certain API keys.

##View API Keys

```php
<?php
// View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
);
$result = $mj->apikey($params);
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
# View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/apikey 
```
```javascript
/**
 *
 * View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("apikey")
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
# View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Apikey.all()
```
```python
"""
View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
"""
from mailjet import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.apikey.get()
```


> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"ACL": "",
			"APIKey": "",
			"CreatedAt": "",
			"ID": "",
			"IsActive": "false",
			"IsMaster": "false",
			"Name": "",
			"QuarantineValue": "",
			"Runlevel": "Normal",
			"SecretKey": "",
			"TrackHost": "",
			"UserID": ""
		}
	],
	"Total": 1
}
```


Perform a <code>GET</code> to view the details of each of your API keys.  Note that if an API key on your account is active (it's able to send emails and access other APIs), it will be shown in the payload from a GET request under 'IsActive' with a value of 'true'.


