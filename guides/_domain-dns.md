#Domains and DNS

To fully take advantage of Mailjet deliverability, you will need to modify your DNS records to include DKIM signature and SPF.

You will also need to verify your sender addresses and domain names. The validation can be done online with the [Sender domains & addresses](https://app.mailjet.com/account/sender) page or through API.

##Create a new DNS entry 

```php
<?php
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


When adding a new sender, if the DNS entry does not exist, the Mailjet system will automatically creates it generating a SPF and DKIM information. 

Use the <code>[/sender](/email-api/v3/sender/)</code> resource to add a new sender. You will get in the response a <code>DNSID</code> you can use to find SPF and DKIM information using the <code>[/dns/$id_or_domain](/email-api/v3/dns/)</code> resource.

You can also add all email addresses for the domain using a catch-all expression like <code>*@yourdomain.com</code> as an <code>Email</code>.

The added email will be inactive except if the domain they belong to has already been validated.

##Get SPF / DKIM settings 

To see the SPF and DKIM for your domain, use a GET request on the <code>[/dns/$id_or_domain](/email-api/v3/dns/)</code> resource using either the <code>DNSID</code> or domain name.

```bash
# View : Sender Domain properties.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/dns/$ID_OR_DOMAINNAME 
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Dns, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"DKIMRecordName": "mailjet._domainkey.example.com",
			"DKIMRecordValue": "k=rsa; p=FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF",
			"DKIMStatus": "OK",
			"Domain": "example.com",
			"ID": "1",
			"IsCheckInProgress": "false",
			"LastCheckAt": "2015-10-30T20:10:08Z",
			"OwnerShipToken": "0123456789abcdef",
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
 - <code>OwnerShipToken</code>: a token, used to validate the domain ownership by the current API key. Validated either by checking the existance of a file at domain root named after this token or a DNS txt record containing this token. 
 - <code>SPFRecordValue</code>: contains the name of the DNS txt record for the SPF configuration. Please note that this field contains the default value, not taking in account existing SPF records for the domain. Concatenating this with the existing value is left up to the API client.
 - <code>SPFStatus</code>: last result of the domain SPF configuration check ran (via the check action or a periodic check on Mailjet side). Status can be "Not checked", "OK", "Error"

Follow the ["How to setup DomainKeys (DKIM) and SPF in my DNS records"](https://www.mailjet.com/support/how-to-setup-domainkeys-dkim-and-spf-in-my-dns-records,84.htm) guide to learn how to use this information to modify your DNS records.

##Check your DNS 

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->post(Resources::$DnsCheck, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
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

##Validate sender and domain

When adding a new sender, you will need to validate this sender to be able to use this email address as a sender. Mailjet will need to verify that you have control over the domain or email address you wish to use.

In the case of a catch-all expression, you will need to do so through one of those 2 options: 

 - adding a text file at the root of you website 
 - adding a TXT record in your domain’s DNS

The text file name can be found in the property <code>Filename</code> when doing a GET on <code>[/sender$id](/email-api/v3/sender/)</code>. You just need to create a file with this name at the root of you website. Mailjet will be looking for http://www.yourdomain.com/[filename].

The TXT record for the DNS can be found in the property <code>OwnerShipTokenRecordName</code> when doing a GET on <code>[/dns/$id_or_domain](/email-api/v3/dns/)</code>. You need to go in your DNS administration interface and add the content of <code>OwnerShipTokenRecordName</code> as a TXT record. The procedure is similar to the <a href="https://www.mailjet.com/support/how-to-setup-domainkeys-dkim-and-spf-in-my-dns-records,84.htm" target="_blank">setup of a SPF record</a>. 

In the case of a standard sender email address, an email will be sent with a validation link. There will be no text file creation or DNS setup necessary and available for validation.

<div></div>

```php
<?php
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


To start the validation, you can call the <code>validate</code> action on the <code>/sender</code> resource.

For a catch-all sender, this synchroneous action will first check for the presence of the proper .txt file at the root of the domain and if not available will check the DNS record for a TXT record.

In the case of a simple sender email address, the validation email will be sent.

The response will contain the method used to validate you domain (<code>ValidationMethod</code>) and an error message (<code>GlobalError</code>).

You will also find a detailed error report per validation method in the <code>Errors</code> property.

The status code returned will be : 

 - 200 : OK (will include success and failure to validate, failure cases will contain only ErrorMessage and no ValidationMethod)
 - 400 : The sender is already active

The <code>ValidationMethod</code> available are : 

 - <code>File</code>: file validation - apply only to catch-all sender
 - <code>DNS</code>: DNS TXT record validation - apply only to catch-all sender
 - <code>Domain</code>: when a validated catch-all sender exists to validate the given standard sender address. 
 - <code>ActivationEmail</code>: sender address activation by confirmation mail - only to standard sender address
