#Domains and DNS

To fully take advantage of Mailjet deliverability, you will need to modify your DNS records to include DKIM signature and SPF.

You will also need to verify your sender addresses and domain names. The validation can be done online with the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> page or through API.

##Create DNS entry

```php
<?php
/*
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Email' => "anothersender@example.com"
];
$response = $mj->post(Resources::$Sender, ['body' => $body]);
$response->success() && var_dump($response->getData());
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
		"Email":"anothersender@example.com"
	}'
```
```javascript
/**
 *
 * Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("sender")
	.request({
		"Email":"anothersender@example.com"
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
# Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Sender.create(email: "anothersender@example.com")
```
```python
"""
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Email': 'anothersender@example.com'
}
result = mailjet.sender.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
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
	var data []resources.Sender
	mr := &Request{
	  Resource: "sender",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Sender {
      Email: "anothersender@example.com",
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
import com.mailjet.client.resource.Sender;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Sender.resource)
						.property(Sender.EMAIL, "anothersender@example.com");
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
			"CreatedAt": "2015-09-07T06:59:52Z",
			"DNSID": "1",
			"Email": "anothersender@example.com",
			"EmailType": "unknown",
			"Filename": "0123456789abcdef.txt",
			"ID": "1",
			"IsDefaultSender": "false",
			"Name": "Myname",
			"Status": "Inactive"
		}
	],
	"Total": 1
}
```


In order to create new sending address, you have to use the resource <code>[/sender](/email-api/v3/sender/)</code>. The domain name will be extracted from the email address and the system will create a <code>DNS</code> resource with it. You can also add all email addresses for the domain using a catch-all expression like <code>*@yourdomain.com</code> as an <code>Email</code>. When querying <code>dns/$id_or_domain</code>, you will find SPF, DKIM and sender validation information.

##Get SPF / DKIM settings 

To see the SPF and DKIM for your domain, use a GET request on the <code>[/dns/$id_or_domain](/email-api/v3/dns/)</code> resource using either the <code>DNSID</code> or domain name.

```php
<?php
/*
View : Sender Domain properties.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Dns, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Sender Domain properties.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/dns/$ID_OR_DOMAINNAME 
```
```javascript
/**
 *
 * View : Sender Domain properties.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("dns")
	.id($ID_OR_DOMAINNAME)
	.request()
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```python
"""
View : Sender Domain properties.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_OR_DOMAINNAME'
result = mailjet.dns.get(id=id)
print result.status_code
print result.json()
```
```ruby
# View : Sender Domain properties.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Dns.find($ID_OR_DOMAINNAME)
```
``` go
/*
View : Sender Domain properties.
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
	var data []resources.Dns
	mr := &Request{
	  Resource: "dns",
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
import com.mailjet.client.resource.Dns;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Sender Domain properties.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Dns.resource, ID);
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
			"DKIMRecordName": "mailjet._domainkey.example.com",
			"DKIMRecordValue": "k=rsa; p=FFFFFFFFFFFFFFFFFFF",
			"DKIMStatus": "OK",
			"Domain": "example.com",
			"ID": "1",
			"IsCheckInProgress": "false",
			"LastCheckAt": "2015-10-30T20:10:08Z",
			"OwnerShipToken": "0123456789abcdef",
			"OwnerShipTokenRecordName": "abcdef0123456789",
			"SPFRecordValue": "v=spf1 include:spf.mailjet.com ?all",
			"SPFStatus": "OK"
		}
	],
	"Total": 1
}
```


Let's have a closer look to some of the response fields:

 - <code>DKIMRecordName</code>: contains the name of the DNS txt record for the DKIM configuration
 - <code>DKIMRecordValue</code>: contains the value of the DNS txt record for the domain DKIM configuration
 - <code>DKIMStatus</code>: last result of the domain DKIM configuration check ran (via the check action or a periodic check on Mailjet side). Status can be "Not checked", "OK", "Error".
 - <code>IsCheckInProgress</code>: indicates if a check is already in progress on Mailjet side. 
 - <code>LastCheckAt</code>: last time a check was run on the given domain (via the check action or a periodic check on Mailjet side)
 - <code>OwnerShipTokenRecordName</code>: provides hostname value for the TXT record, used to validate sending domain
 - <code>OwnerShipToken</code>: provides TXT record value, for the TXT record, used for sending validation 
 - <code>SPFRecordValue</code>: contains the name of the DNS txt record for the SPF configuration. Please note that this field contains the default value, not taking in account existing SPF records for the domain. Concatenating this with the existing value is left up to the API client.
 - <code>SPFStatus</code>: last result of the domain SPF configuration check ran (via the check action or a periodic check on Mailjet side). Status can be "Not checked", "OK", "Error"

Follow the <a href="https://www.mailjet.com/docs/spf-dkim-guide" target="_blank">"How to setup DomainKeys (DKIM) and SPF in my DNS records"</a> guide to learn how to use this information to modify your DNS records.

##Check your DNS 

```bash
# Check : Run a check on a domain
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/dns/$ID_OR_DOMAINNAME/check \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Check : Run a check on a domain
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("dns")
	.id($ID_OR_DOMAINNAME)
	.action("check")
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
# Check : Run a check on a domain
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Dns_check.create(id: $ID_OR_DOMAINNAME)
```
```php
<?php
/*
Check : Run a check on a domain
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$DnsCheck, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```python
"""
Check : Run a check on a domain
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_OR_DOMAINNAME'
result = mailjet.dns_check.create(id=id)
print result.status_code
print result.json()
```
``` go
/*
Check : Run a check on a domain
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
	var data []resources.DnsCheck
	mr := &Request{
	  Resource: "dns",
	  ID: RESOURCE_ID,
	  Action: "check",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.DnsCheck {
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
import com.mailjet.client.resource.DnsCheck;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Check : Run a check on a domain
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(DnsCheck.resource, ID);
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```


To help you validate your DNS configuration, our API provides you with a resource action which reads the domain DNS records and try to match them with the expected configuration on Mailjet side. 

You can verify that your DNS setup is conform to the necessary modifications by issuing a POST request on <code>[/dns/$ID/check](/email-api/v3/dns-check/)</code>

<div></div>

> API response:

```json
{
    "Count": 1,
    "Data": [
        {
            "DKIMErrors": [ "no DKIM record" ],
            "DKIMRecordCurrentValue": "", 
            "DKIMStatus": "Error", 
            "SPFErrors": [],
            "SPFRecordsCurrentValues": [ "v=spf1 include:spf.mailjet.com -all" ], 
            "SPFStatus": "OK" 
        }
    ],
    "Total": 1
}
```


The result of the check will contain the following information : 

 - <code>DKIMErrors</code>: detail of error(s) on DKIM configuration 
 - <code>DKIMRecordCurrentValue</code>: the value retrieved from the check
 - <code>DKIMStatus</code>: possible values are <code>Not checked</code>, <code>OK</code>, <code>Error</code>
 - <code>SPFErrors</code>: detail of error(s) on SPF configuration
 - <code>SPFRecordsCurrentValues</code>: the values retrieved from the check
 - <code>SPFStatus</code>: possible values are <code>Not checked</code>, <code>OK</code>, <code>Error</code>

You can see if SPF and DKIM are ready to use Mailjet.

The check action follows the SPF records in cascade and takes CNAME records into account.

##Validate a sender

###Validate single sending address

```php
<?php
/*
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Email' => "anothersender@example.com"
];
$response = $mj->post(Resources::$Sender, ['body' => $body]);
$response->success() && var_dump($response->getData());
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
		"Email":"anothersender@example.com"
	}'
```
```javascript
/**
 *
 * Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("sender")
	.request({
		"Email":"anothersender@example.com"
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
# Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Sender.create(email: "anothersender@example.com")
```
```python
"""
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Email': 'anothersender@example.com'
}
result = mailjet.sender.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
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
	var data []resources.Sender
	mr := &Request{
	  Resource: "sender",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Sender {
      Email: "anothersender@example.com",
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
import com.mailjet.client.resource.Sender;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Sender.resource)
						.property(Sender.EMAIL, "anothersender@example.com");
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
			"CreatedAt": "2015-09-07T06:59:52Z",
			"DNSID": "1",
			"Email": "anothersender@example.com",
			"EmailType": "unknown",
			"Filename": "0123456789abcdef.txt",
			"ID": "1",
			"IsDefaultSender": "false",
			"Name": "Myname",
			"Status": "Inactive"
		}
	],
	"Total": 1
}
```


In order to authorize sending email address in Mailjet, use the <code>[/sender](/email-api/v3/sender/)</code> resource. The operation will return <code>ID</code> in the response.

Once added, the email will be inactive by default. In order to activate it, call <code>sender/$ID/validate</code>, substituting the <code>$ID</code> with the relevant sender ID. The successful operation will trigger confirmation email, sent to the email address, you want to activate. Once you click on the activation link, the email address will be ready to go.

<div></div>

```php
<?php
/*
Validate : check if the Ownership token has been properly setup on the website or DNS
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$SenderValidate, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# Validate : check if the Ownership token has been properly setup on the website or DNS
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/sender/$ID/validate \
	-H 'Content-Type: application/json' \
	-d '{
	}'
```
```javascript
/**
 *
 * Validate : check if the Ownership token has been properly setup on the website or DNS
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("sender")
	.id($ID)
	.action("validate")
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
# Validate : check if the Ownership token has been properly setup on the website or DNS
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Sender_validate.create(id: $ID)
```
```python
"""
Validate : check if the Ownership token has been properly setup on the website or DNS
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID'
result = mailjet.sender_validate.create(id=id)
print result.status_code
print result.json()
```
``` go
/*
Validate : check if the Ownership token has been properly setup on the website or DNS
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
	var data []resources.SenderValidate
	mr := &Request{
	  Resource: "sender",
	  ID: RESOURCE_ID,
	  Action: "validate",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.SenderValidate {
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
import com.mailjet.client.resource.SenderValidate;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Validate : check if the Ownership token has been properly setup on the website or DNS
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(SenderValidate.resource, ID);
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
			"Errors": {
				"DNSValidationError": " DNS record verification failed as no TXT record for that ownership token was found on the remote server.",
				"FileValidationError": " File verification failed as ecff63dc3593b2323b5ab3d7d823c103.txt cannot be found at domain root."
			},
			"GlobalError": "Neither the file at your domain root nor the DNS TXT record have been found to validate this sender.",
			"ValidationMethod": ""
		}
	],
	"Total": 1
}
```


###Validate sending domain

You can also authorize all email addresses for a whole domain, using a catch-all expression. You should provide a value <code>*@yourdomain.com</code> for the <code>Email</code> property for <code>/sender</code> resource. The API response will contain property called <code>DNSID</code>. Use the ID to query the resource <code>dns/$id</code>. The output will contain information about the available validation methods.

####DNS validation using TXT record
You can activate the domain by creating a TXT record in it's DNS zone file. The TXT record should contain the following values:

 - Hostname: contains the value of the property <code>OwnerShipTokenRecordName</code>, returned when querying <code>dns/$id</code>
 - Record value: contains the value of the property <code>OwnerShipToken</code>, returned when querying <code>dns/$id</code>

The valid TXT record has the following format:
<code>OwnerShipTokenRecordName.yourdomain.com. IN TXT 300 OwnerShipToken</code>

Once the record is live, you can call <code>sender/$id/validate</code> to activate the sending domain. Keep in mind that here you need to use the sender ID and not the DNS id.

The process of TXT record creation for sender validation is similar to the TXT record creation for SPF and DKIM. Please refer to <a href="https://www.mailjet.com/docs/spf-dkim-guide" target="_blank">"How to setup DomainKeys (DKIM) and SPF in my DNS records"</a> guide for details.

####Validation using TXT file upload

Another option to authorize a domain will be to upload a file to your website. The text file name can be found in the property <code>Filename</code> when doing a GET on <code>/sender/$id</code>. You just need to create a file with this name at the root of you website. Mailjet will be looking for http://www.yourdomain.com/[filename].

The response will contain the method used to validate you domain (<code>ValidationMethod</code>) and an error message (<code>GlobalError</code>).

####Error handling

You can find a detailed error report per validation method in the <code>Errors</code> property.

The status code returned will be : 

 - 200 : OK (will include success and failure to validate, failure cases will contain only ErrorMessage and no ValidationMethod)
 - 400 : The sender is already active

The <code>ValidationMethod</code> available are : 

 - <code>File</code>: file validation - apply only to catch-all sender
 - <code>DNS</code>: DNS TXT record validation - apply only to catch-all sender
 - <code>Domain</code>: when a validated catch-all sender exists to validate the given standard sender address. 
 - <code>ActivationEmail</code>: sender address activation by confirmation mail - only to standard sender address
