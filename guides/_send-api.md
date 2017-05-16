#Send transactional email
##Choose sending method

Mailjet offers two ways to send transactional emails : SMTP Relay or Send API

###SMTP Relay

<a href="https://www.mailjet.com/feature/smtp-relay/" target="_blank">SMTP Relay</a> to send email in a fairly easy way, requiring minimum integration effort on your side.

The SMTP Relay is useful if you have an existing solution for transactional email by SMTP or if you cannot use the Send API. Using the SMTP relay, you have to take care of email headers, MIME type handling and completely format and personalize your message content. 

The best and fastest way to use the SMTP Relay is to have your own local mail server relaying messages to the Mailjet SMTP. Your local mail server will give you reliable management of the messages and connections between our 2 systems. 
In case of breakage in the connection, your mail server will handle properly the error and retry to send your messages. 

In case you don't have a local mail server, you can still use the SMTP Relay by using one of the many SMTP libraries available or configuring your exiting system (frameworks, CMS, CRM...). However, some of these libraries or systems can lack the advanced error handling necessary to queue and resend the messages in case of error. The use of Send API can be a better choice has it require less interactions between our systems and limits the risk of failures. The error handling is also a lot simpler with the API as we are managing for you the delivery and queuing of your messages.

Using Mailjet's SMTP servers to send your transactional emails is very simple. All you have to do is update your SMTP server settings to use our server as a "relay" or "smarthost" with the credentials provided by Mailjet. The credentials are your $MJ_APIKEY_PUBLIC as a login and $MJ_APIKEY_PRIVATE as a password.

You can find your SMTP credentials in your <a href="https://eu.mailjet.com/account/setup" target="_blank">Account Setup page</a>

You can find more information on how to configure the SMTP Relay and custom email headers in our [SMTP Relay Guide](#SMTP_Relay_Use).


### Send API

Mailjet's Send API allows you to send transactional emails using our HTTP API, using POST requests. This solution is aimed at users needing a programmatically way to send messages.

Send API allows to send single messages but also to mutualise the calls by leveraging templating and personalisation of the content. 

The API will return a simple response indicating if the message is ready to be processed on the Mailjet system. This makes the error management on your side simple and efficient. 


##Verify a Sender

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
      /// Create : Manage an email sender for a single API key. An e-mail address or a complete domain (*) has to be registered and validated before being used to send e-mails. In order to manage a sender available across multiple API keys, see the related MetaSender resource.
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
            Resource = Sender.Resource,
         }
            .Property(Sender.Email, "anothersender@example.com");
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


To create a sender, provide the email address of the sender as part of a <code>POST</code> on the resource <code>[/sender](/email-api/v3/sender/)</code>.

A verification email will be sent to the address you added to activate this new sender.

<aside class="notice">The validation of a sender address can also be initiated with API calls. Visit the <a href="#domains-and-dns">Domains and DNS</a> guide for more information.</aside>

##Setup SPF/DKIM on DNS 

To increase the deliverability of your emails, dont forget to setup properly your DNS record. 

You can either visit [the Mailjet user interface](#verify-your-domain) or use the [Domains and DNS](#domains-and-dns) API guide to setup SPF and DKIM.


##Sending a basic email

``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
	'FromEmail': 'pilot@mailjet.com',
	'FromName': 'Mailjet Pilot',
	'Subject': 'Your email flight plan!',
	'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
	'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
	'Recipients': [{'Email':'passenger@mailjet.com'}]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This calls sends an email to one recipient.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
	}
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet Pilot",
		subject: "Your email flight plan!",
		text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		recipients: [{ 'Email'=> 'passenger@mailjet.com'}])
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to one recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```bash
# This calls sends an email to one recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
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
      /// This calls sends an email to one recipient.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
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


To send an email, you need the following mandatory properties: 

 - <code>FromEmail</code>: a verified Sender address 
 - <code>Recipients</code>: list with at least one <code>Email</code> address 
 - <code>Text-part</code> or/and <code>Html-part</code>: content of the message sent in text or HTML format. At least one of these content type needs to be specified. When Html-part is the only content provided, Mailjet will not generate a Text-part from the HTML version.   

Optionally, in place of <code>Recipients</code>, you can use <code>To</code>, <code>Cc</code> and <code>Bcc</code> properties. To, Cc and Bcc can't be used in conjunction with <code>Recipients</code>. The properties can contain several recipients separated by comma using the following format <code>john@example.com</code>, <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code>. 


Important: <code>Recipients</code> and <code>To</code> have a different behaviors. The recipients listed in <code>To</code> will receive a common message showing every other recipients and carbon copies recipients. The recipients listed in <code>Recipients</code> will each receive an separate message without showing all the other recipients. 


<div></div>
> API response:

```json
{
	"Sent": [
		{
			"Email": "passenger@mailjet.com",
			"MessageID": 111111111111111
		}
	]
}
```
<div></div>
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'To' => "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",
    'CC' => "Name3 <passenger@mailjet.com>"
];
$response = $mj->post(Resources::$Send, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# This calls sends an email to two recipients in To and one recipient in CC.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"To":"Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",
		"CC":"Name3 <passenger@mailjet.com>"
	}'
```
```javascript
/**
 *
 * This calls sends an email to two recipients in To and one recipient in CC.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"To":"Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",
		"CC":"Name3 <passenger@mailjet.com>"
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
# This calls sends an email to two recipients in To and one recipient in CC.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(from_email: "pilot@mailjet.com",from_name: "Mailjet Pilot",subject: "Your email flight plan!",text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",to: "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",cc: "Name3 <passenger@mailjet.com>")
```
```python
"""
This calls sends an email to two recipients in To and one recipient in CC.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  'To': 'Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>',
  'CC': 'Name3 <passenger@mailjet.com>'
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This calls sends an email to two recipients in To and one recipient in CC.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      To: "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",
      CC: "Name3 <passenger@mailjet.com>",
    },
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to two recipients in To and one recipient in CC.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.TO, "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>")
						.property(Email.CC, "Name3 <passenger@mailjet.com>");
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
      /// This calls sends an email to two recipients in To and one recipient in CC.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.To, "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>")
            .Property(Send.Cc, "Name3 <passenger@mailjet.com>");
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


When using  <code>To</code>, <code>Cc</code> and <code>Bcc</code> instead of <code>Recipients</code>, the email addresses need to be presented as SMTP headers in a string and not as an array.
<div></div>


<code>MessageID</code> is the internal Mailjet id of your message. You will be able to use this id to get more information about your message. You can find more information in our [Message Statistics Guide](#messages)


<aside class="notice">
NOTICE: If a recipient does not exist in any of your contact list it will be created from scratch, keep that in mind if you are planning on sending a welcome email and then you're trying to add the email to a list as the contact effectively exists already.
</aside>

##Sending to multiple recipients

``` python
"""
This calls sends an email to 2 recipients.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  'Recipients': [
		{
			"Email": "passenger1@mailjet.com",
			"Name": "passenger 1"
		},
		{
			"Email": "passenger2@mailjet.com",
			"Name": "passenger 2"
		}
	]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to 2 recipients.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger1@mailjet.com")
                    .put("Name", "passenger 1"))
                .put(new JSONObject()
                    .put("Email", "passenger2@mailjet.com")
                    .put("Name", "passenger 2")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger1@mailjet.com",
            'Name' => "passenger 1"
        ],
        [
            'Email' => "passenger2@mailjet.com",
            'Name' => "passenger 2"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet Pilot",
		subject: "Your email flight plan!",
		text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com', 'Name' => 'passenger 1'}, {'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}])
```
```bash
# This calls sends an email to 2 recipients.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
``` go
/*
This calls sends an email to 2 recipients.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger1@mailjet.com",
          Name: "passenger 1",
        },
        Recipient {
          Email: "passenger2@mailjet.com",
          Name: "passenger 2",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
	}
}
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
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
      /// This calls sends an email to 2 recipients.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger1@mailjet.com"},
                 {"Name", "passenger 1"}
                 },
                new JObject {
                 {"Email", "passenger2@mailjet.com"},
                 {"Name", "passenger 2"}
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
	"Sent": [
		{
			"Email": "passenger1@mailjet.com",
			"MessageID": 20547681647433000
		},
		{
			"Email": "passenger2@mailjet.com",
			"MessageID": 20547681647433001
		}
	]
}
```

To send the same email to multiple contacts, add <code>Email</code> addresses in the <code>Recipients</code> list.

Each recipient will receive a dedicated message.

##Sending with attached files

``` python
"""
This calls sends an email to the given recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3>May the delivery force be with you!',
  'Recipients': [{ "Email": "passenger@mailjet.com"}],
  'Attachments':
		[{
			"Content-type": "text/plain",
			"Filename": "test.txt",
			"content": "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
		}]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to the given recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.ATTACHMENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Content-type", "text/plain")
                    .put("Filename", "test.txt")
                    .put("content", "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK")));
      response = client.post(request);
      System.out.println(response.getData());
    }
}
```
``` go
/*
This calls sends an email to the given recipient.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      Attachments: []MailjetAttachment {
        MailjetAttachment {
          ContentType: "text/plain",
          Filename: "test.txt",
          Content: "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
	}
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Attachments' => [
        [
            'Content-type' => "text/plain",
            'Filename' => "test.txt",
            'content' => "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
# This calls sends an email to the given recipient.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet Pilot",
		subject: "Your email flight plan!",
		text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
    attachments: [{'Content-Type' => 'text/plain', 'Filename' => 'test.txt', 'content' => 'VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK'}])
```
```bash
# This calls sends an email to the given recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Attachments":[{"Content-type":"text/plain","Filename":"test.txt","content":"VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Attachments":[{"Content-type":"text/plain","Filename":"test.txt","content":"VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log (err.statusCode)
	})
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
      /// This calls sends an email to the given recipient.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.Attachments, new JArray {
                new JObject {
                 {"Content-type", "text/plain"},
                 {"Filename", "test.txt"},
                 {"content", "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"}
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


To attach files, use <code>Attachments</code> or <code>Inline_attachments</code>.  
The recipient of a email with attachment will have to click to see it. The inline attachment can be visible directly in the body of the message depending of the email client support.  

In both calls, the content will need to be Base64 encoded. You will need to specify the MIME type and a file name.

<div></div>

``` python
"""
This calls sends an email to the given recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to <img src="cid:logo.gif">Mailjet!</h3><br />May the delivery force be with you!',
  'Recipients': [ { "Email": "passenger@mailjet.com" }],
  'Inline_attachments': [
				{
						"Content-type": "image/gif",
						"Filename": "logo.gif",
						"content": "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"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to the given recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.INLINE_ATTACHMENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Content-type", "image/gif")
                    .put("Filename", "logo.gif")
                    .put("content", "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")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Inline_attachments' => [
        [
            'Content-type' => "image/gif",
            'Filename' => "logo.gif",
            'content' => "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"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet Pilot",
		subject: "Your email flight plan!",
		text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		html_part: "<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
    'inline-attachments' => [{'Content-type' => 'image/gif', 'Filename' => 'logo.gif', 'content' => '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'}])
```
```bash
# This calls sends an email to the given recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Inline_attachments":[{"Content-type":"image/gif","Filename":"logo.gif","content":"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"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Inline_attachments":[{"Content-type":"image/gif","Filename":"logo.gif","content":"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"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(error => {
    console.log(error.statusCode)
	})
```
``` go
/*
This calls sends an email to the given recipient.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      InlineAttachments: []MailjetAttachment {
        MailjetAttachment {
          ContentType: "image/gif",
          Filename: "logo.gif",
          Content: "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",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to the given recipient.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <img src=\"cid:logo.gif\">Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.InlineAttachments, new JArray {
                new JObject {
                 {"Content-type", "image/gif"},
                 {"Filename", "logo.gif"},
                 {"content", "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"}
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


When using an inline Attachment, it's possible to insert the file inside the HTML code of the email by using <code>cid:FILENAME.EXT</code> where FILENAME.EXT is the <code>Filename</code> specified in the declaration of the Attachment.

<aside class="warning">
Remember to keep the size of your attachements low and not to exceed 15 MB.
</aside>


##Sending in bulk

``` python
"""
This calls sends an email to the given recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Messages': [
	 {
		"FromEmail": "pilot@mailjet.com",
		"FromName": "Mailjet Pilot",
		"Recipients": [{ "Email": "passenger1@mailjet.com", "Name": "passenger 1" }],
		"Subject": "Your email flight plan!",
		"Text-part": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
		"Html-part": "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
	  },
	  {
		"FromEmail": "pilot@mailjet.com",
		"FromName": "Mailjet Pilot",
		"Recipients": [{ "Email": "passenger2@mailjet.com", "Name": "passenger 2" }],
		"Subject": "Your email flight plan!",
		"Text-part": "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
		"Html-part": "<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!"
	  }
	]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to the given recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put("FromEmail", "pilot@mailjet.com")
                    .put("FromName", "Mailjet Pilot")
                    .put("Recipients", new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put("Subject", "Your email flight plan!")
                    .put("Text-part", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put("Html-part", "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"))
                .put(new JSONObject()
                    .put("FromEmail", "pilot@mailjet.com")
                    .put("FromName", "Mailjet Pilot")
                    .put("Recipients", new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger2@mailjet.com")
                            .put("Name", "passenger 2")))
                    .put("Subject", "Your email flight plan!")
                    .put("Text-part", "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!")
                    .put("Html-part", "<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Messages' => [
        [
            'FromEmail' => "pilot@mailjet.com",
            'FromName' => "Mailjet Pilot",
            'Recipients' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'Text-part' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'Html-part' => "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
        ],
        [
            'FromEmail' => "pilot@mailjet.com",
            'FromName' => "Mailjet Pilot",
            'Recipients' => [
                [
                    'Email' => "passenger2@mailjet.com",
                    'Name' => "passenger 2"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'Text-part' => "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
            'Html-part' => "<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  messages: [
    {
      'FromEmail' => 'pilot@mailjet.com',
      'FromName' => 'Mailjet Pilot',
      'Recipients' => [{'Email' => 'passenger1@mailjet.com', 'Name' => 'passenger 1'}],
      'subject' => 'email 1',
      'text-part' => 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    },
    {
      'fromEmail' => 'pilot@mailjet.com',
      'fromName' => 'Mailjet Pilot',
      'Recipients' => [{'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}],
      'subject' => 'email 2',
      'text-part' => 'Dear passenger 2, welcome to Mailjet! May the delivery force be with you!',
    }])
```
```bash
# This calls sends an email to the given recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
			},
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger2@mailjet.com","Name":"passenger 2"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!"
			}
		]
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"Messages":[
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
			},
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger2@mailjet.com","Name":"passenger 2"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!"
			}
		]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(error => {
		console.log (error.statusCode);
	})
```
``` go
/*
This calls sends an email to the given recipient.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      Messages: []InfoSendMail {
        InfoSendMail {
          FromEmail: "pilot@mailjet.com",
          FromName: "Mailjet Pilot",
          Recipients: []Recipient {
            Recipient {
              Email: "passenger1@mailjet.com",
              Name: "passenger 1",
            },
          },
          Subject: "Your email flight plan!",
          TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
          HTMLPart: "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!",
        },
        InfoSendMail {
          FromEmail: "pilot@mailjet.com",
          FromName: "Mailjet Pilot",
          Recipients: []Recipient {
            Recipient {
              Email: "passenger2@mailjet.com",
              Name: "passenger 2",
            },
          },
          Subject: "Your email flight plan!",
          TextPart: "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
          HTMLPart: "<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to the given recipient.
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
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"FromEmail", "pilot@mailjet.com"},
                 {"FromName", "Mailjet Pilot"},
                 {"Recipients", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"Text-part", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"Html-part", "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"}
                 },
                new JObject {
                 {"FromEmail", "pilot@mailjet.com"},
                 {"FromName", "Mailjet Pilot"},
                 {"Recipients", new JArray {
                  new JObject {
                   {"Email", "passenger2@mailjet.com"},
                   {"Name", "passenger 2"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"Text-part", "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!"},
                 {"Html-part", "<h3>Dear passenger 2, welcome to Mailjet!</h3><br />May the delivery force be with you!"}
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
	"Sent": [
		{
			"Email": "passenger1@mailjet.com",
			"MessageID": 20547681647433000
		},
		{
			"Email": "passenger2@mailjet.com",
			"MessageID": 20547681647433001
		}
	]
}
```
To send messages in bulk, package the multiple messages inside a <code>Messages</code> property. Each message inside this list of message will enjoy the same properties described above. 

##Personalisation
<div></div>
###Content formatting

Mailjet system allows to insert data in your text or html parts. 

To do so, use <code>[[DATA_TYPE:DATA_NAME]]</code> where: 

- <code>DATA_TYPE</code>: <code>var</code> for Vars specified in the API call or <code>data</code> for contact data already available on Mailjet system 
- <code>DATA_NAME</code>: name of the data you want to insert

<div></div>
###Using vars and custom vars 

``` python
"""
This calls sends an email to 2 given recipient with global personalisation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!',
  'Vars': {
			"day": "Monday"
		},
  'Recipients': [
		{
			"Email": "passenger1@mailjet.com",
			"Name": "passenger 1"
		},
		{
			"Email": "passenger2@mailjet.com",
			"Name": "passenger 2"
		}
	]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to 2 given recipient with global personalisation.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! On this [[var:day:\"monday\"]], may the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day:\"monday\"]], may the delivery force be with you!")
						.property(Email.VARS, new JSONObject()
                .put("day", "Monday"))
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger1@mailjet.com")
                    .put("Name", "passenger 1"))
                .put(new JSONObject()
                    .put("Email", "passenger2@mailjet.com")
                    .put("Name", "passenger 2")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
/*
This calls sends an email to 2 given recipient with global personalisation.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!",
    'Vars' => [
        'day' => "Monday"
    ],
    'Recipients' => [
        [
            'Email' => "passenger1@mailjet.com",
            'Name' => "passenger 1"
        ],
        [
            'Email' => "passenger2@mailjet.com",
            'Name' => "passenger 2"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  'FromEmail' => 'pilot@mailjet.com',
  'FromName' => 'Mailjet Pilot',
  'Subject' => 'Your email flight plan!',
  'Text-part' => 'Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!',
  'Html-part' => '<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!',
  'Vars' => {'day' => 'Monday'},
  'Recipients' => [
      {'Email' => 'passenger1@mailjet.com','Name' => 'passenger 1'},
      {'Email' => 'passenger2@mailjet.com','Name' => 'passenger 2'}
  ]
)
```
```bash
# This calls sends an email to 2 given recipient with global personalisation.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to 2 given recipient with global personalisation.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
  .catch(e => {
		console.log(e.statusCode)
	})
```
``` go
/*
This calls sends an email to 2 given recipient with global personalisation.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
type  MyVarsStruct  struct {
  Day  string
}
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!",
      Vars: MyVarsStruct {
        Day: "Monday",
      },
      Recipients: []Recipient {
        Recipient {
          Email: "passenger1@mailjet.com",
          Name: "passenger 1",
        },
        Recipient {
          Email: "passenger2@mailjet.com",
          Name: "passenger 2",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to 2 given recipient with global personalisation.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you!")
            .Property(Send.Vars, new JObject {
                {"day", "Monday"}
                })
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger1@mailjet.com"},
                 {"Name", "passenger 1"}
                 },
                new JObject {
                 {"Email", "passenger2@mailjet.com"},
                 {"Name", "passenger 2"}
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


By using <code>Vars</code> in conjunction with the <code>[[var:VAR_NAME]]</code>, you can modify the content of you email.

<div></div>

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to 2 recipients with global and contact personalisation.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! On this [[var:day:\"monday\"]], may the delivery force be with you! [[var:personalmessage:\"\"]]")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day:\"monday\"]], may the delivery force be with you! [[var:personalmessage:\"\"]]")
						.property(Email.VARS, new JSONObject()
                .put("day", "Monday"))
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger1@mailjet.com")
                    .put("Name", "passenger 1")
                    .put("Vars", new JSONObject()
                        .put("day", "Tuesday")
                        .put("personalmessage", "Happy birthday!")))
                .put(new JSONObject()
                    .put("Email", "passenger2@mailjet.com")
                    .put("Name", "passenger 2")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
"""
This calls sends an email to 2 recipients with global and contact personalisation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]',
  'Vars': {
		"day": "Monday"
  },
  'Recipients': [
		{
			"Email": "passenger1@mailjet.com",
			"Name": "passenger 1",
			"Vars": {
				"day": "Tuesday",
				"personalmessage": "Happy birthday!"
			}
		},
		{
			"Email": "passenger2@mailjet.com",
			"Name": "passenger 2"
		}
	]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```php
<?php
/*
This calls sends an email to 2 recipients with global and contact personalisation.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
    'Vars' => [
        'day' => "Monday"
    ],
    'Recipients' => [
        [
            'Email' => "passenger1@mailjet.com",
            'Name' => "passenger 1",
            'Vars' => [
                'day' => "Tuesday",
                'personalmessage' => "Happy birthday!"
            ]
        ],
        [
            'Email' => "passenger2@mailjet.com",
            'Name' => "passenger 2"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: "pilot@mailjet.com",
  from_name: "Mailjet Pilot",
  subject: "Your email flight plan!",
  text_part: "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
  html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
  vars: {'day' => 'Monday'},
  recipients: [
    {'Email' => 'passenger1@mailjet.com', 'Name' => 'passenger 1', 'Vars' => {'day' => 'Tuesday', 'personalmessage' => 'Happy birthday!'}},
    {'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}]
)
```
```bash
# This calls sends an email to 2 recipients with global and contact personalisation.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1","Vars":{"day":"Tuesday","personalmessage":"Happy birthday!"}},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to 2 recipients with global and contact personalisation.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1","Vars":{"day":"Tuesday","personalmessage":"Happy birthday!"}},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
  .catch(error => {
		console.log(error.statusCode)
	})
```
``` go
/*
This calls sends an email to 2 recipients with global and contact personalisation.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
type  MyVarsStruct  struct {
  Day  string
}
type MyRecipientsVarsStruct struct {
  Day               string
  Personalmessage   string
}
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
      Vars: MyVarsStruct {
        Day: "Monday",
      },
      Recipients: []Recipient {
        Recipient {
          Email: "passenger1@mailjet.com",
          Name: "passenger 1",
          Vars: MyRecipientsVarsStruct {
            Day: "Tuesday",
            Personalmessage: "Happy birthday!",
          },
        },
        Recipient {
          Email: "passenger2@mailjet.com",
          Name: "passenger 2",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to 2 recipients with global and contact personalisation.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]")
            .Property(Send.Vars, new JObject {
                {"day", "Monday"}
                })
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger1@mailjet.com"},
                 {"Name", "passenger 1"},
                 {"Vars", new JObject {
                  {"day", "Tuesday"},
                  {"personalmessage", "Happy birthday!"}
                  }}
                 },
                new JObject {
                 {"Email", "passenger2@mailjet.com"},
                 {"Name", "passenger 2"}
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


To go further in personalisation <code>Vars</code> is also available for each recipient. The main <code>Vars</code> will be overidden by the recipient <code>Vars</code>


<div></div>
###Using contact properties

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to the given recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger1@mailjet.com")
                    .put("Name", "passenger 1"))
                .put(new JSONObject()
                    .put("Email", "passenger2@mailjet.com")
                    .put("Name", "passenger 2")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
	"FromEmail":"pilot@mailjet.com",
	"FromName":"Mailjet Pilot",
	"Subject":"Your email flight plan!",
	"Text-part":"Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!",
	"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!",
	"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger1@mailjet.com",
            'Name' => "passenger 1"
        ],
        [
            'Email' => "passenger2@mailjet.com",
            'Name' => "passenger 2"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: 'pilot@mailjet.com',
  from_name: 'Mailjet Pilot',
  subject: 'Your email flight plan!',
  text_part: 'Dear [[data:firstname: "passenger"]], welcome to Mailjet! May the delivery force be with you!',
  html_part: '<h3>Dear [[data:firstname: "passenger"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!',
  recipients:[
    {'Email':'passenger1@mailjet.com','Name':'passenger 1'},
    {'Email':'passenger2@mailjet.com','Name':'passenger 2'}])
```
```bash
# This calls sends an email to the given recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!",
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!",
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
```
``` go
/*
This calls sends an email to the given recipient.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger1@mailjet.com",
          Name: "passenger 1",
        },
        Recipient {
          Email: "passenger2@mailjet.com",
          Name: "passenger 2",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to the given recipient.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear [[data:firstname:\"passenger\"]], welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear [[data:firstname:\"passenger\"]], welcome to Mailjet!</h3><br /> May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger1@mailjet.com"},
                 {"Name", "passenger 1"}
                 },
                new JObject {
                 {"Email", "passenger2@mailjet.com"},
                 {"Name", "passenger 2"}
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


If the contact you are sending an email to is already in Mailjet system with some contact datas, you can leverage this information to personalise your email.

Use <code>[[data:METADATA_NAME]]</code> or <code>[[data:METADATA_NAME:DEFAULT_VALUE]]</code> to insert data in your content.

<code>DEFAULT_VALUE</code>is the default value that will be used if no data found.

[More information](#personalisation-add-contact-properties) about how to add contact properties.

###Go further with personalisation

Mailjet offers a Templating language that can extend the personalisation of your transactional messages. 
Visit [Transactional templating](#transactional-templating) guide to learn about additional substitutions, modification functions and conditional statements you can use to personalize messages.


## Using a Template

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.errors.MailjetClient;
import com.mailjet.client.errors.MailjetRequest;
import com.mailjet.client.errors.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends a message based on a template.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.MJTEMPLATEID, 1)
						.property(Email.MJTEMPLATELANGUAGE, true)
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.MJEVENTPAYLOAD, "Eticket,1234,row,15,seat,B");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
/*
This calls sends a message based on a template.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'MJ-TemplateID' => 1,
    'MJ-TemplateLanguage' => true,
    'Recipients' => [['Email' => "passenger@mailjet.com"]]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# This calls sends a message based on a template.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"MJ-TemplateID":"1",
		"MJ-TemplateLanguage":true,
		"Recipients":[
				{
						"Email": "passenger@mailjet.com"
				}
		]
	}'
```
```javascript
/**
 *
 * This calls sends a message based on a template.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"MJ-TemplateID":"1",
		"MJ-TemplateLanguage":"true",
		"Recipients":[
				{
						"Email": "passenger@mailjet.com"
				}
		]
	});
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
```
```ruby
# This calls sends a message based on a template.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: "pilot@mailjet.com",
  from_name: "Mailjet Pilot",
  subject: "Your email flight plan!",
  "Mj-TemplateID": "1",
  "Mj-TemplateLanguage": true,
  recipients: [{ 'Email'=> 'passenger@mailjet.com'}]
)
```
```python
"""
This calls sends a message based on a template.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'MJ-TemplateID': '1',
  'MJ-TemplateLanguage': 'true',
  'Recipients': [
				{
						"Email": "passenger@mailjet.com"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This calls sends a message based on a template.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      MjTemplateID: "1",
      MjTemplateLanguage: "true",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends a message based on a template.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.MjTemplateID, "1")
            .Property(Send.MjTemplateLanguage, "True")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
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


Mailjet offers to store your transactional message templates on its platform. You can use these templates in your transactional API call to remove the need to repeat the content of a transactional message at each Send API call. 

You can either create the templates through our online drag and drop tool [Passport](https://www.mailjet.com/feature/passport/) or through the [/template](/email-api/v3/template/) resource. 

You can learn more about storing your templates through API [here](#storing-a-template).

You can also follow our [Step by Step guide](#step-by-step-guide) to create your first Passport template with templating language.

In the templates, you will be able to use simple [personalisation](#personalisation) (<code>[[data:property_name]]</code> or <code>[[var:variable_name]]</code>) or advanced [templating language](#template-api) (<code>{{data:property_name}}</code>, <code>{{var:variable_name}}</code>, conditional statements and loop statements)

In this sample, <code>Mj-TemplateID</code> will be the Id provided by Passport at the end of your designing process or the Id returned by the <code>/template</code> resource. 

<aside class="notice">
The <code>Mj-TemplateLanguage</code> property in the payload provided to send API is optional but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

## Using Template Language

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.errors.MailjetClient;
import com.mailjet.client.errors.MailjetRequest;
import com.mailjet.client.errors.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to one recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
            .property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
            .property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
						.property(Email.MJTEMPLATELANGUAGE, true)
            .put(Email.VARS, new JSONObject().put("Day", "Monday"))
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")
                    .put("Vars", new JSONObject().put("Day", "Tuesday"))))
                .put(new JSONObject()
                    .put("Email", "passenger2@mailjet.com"))
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'MJ-TemplateLanguage' => true,
    'Text-part' => 'Dear passenger, welcome to Mailjet! On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}'
    'Html-part' => '<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}',
    'Recipients' => [
      ['Email' => 'passenger2@mailjet.com', 'Name' => 'Passenger 2'],
      ['Email' => 'passenger@mailjet.com', 'Name' => 'passenger 1',
        'Vars' => ['day' => 'Tuesday', 'Personalmessage' => 'Happy Birthday!']]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# This calls sends an email to the given recipient with vars and custom vars.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"MJ-TemplateLanguage":true,
		"Text-part":"Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Vars":{
				"day": "Monday"
		},
		"Recipients":[
				{
						"Email": "passenger1@mailjet.com",
						"Name": "passenger 1",
						"Vars": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				},
				{
						"Email": "passenger2@mailjet.com",
						"Name": "passenger 2"
				}
		]
	}'
```
```javascript
/**
 *
 * This calls sends an email to the given recipient with vars and custom vars.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"MJ-TemplateLanguage":"true",
		"Text-part":"Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Vars":{
				"day": "Monday"
		},
		"Recipients":[
				{
						"Email": "passenger1@mailjet.com",
						"Name": "passenger 1",
						"Vars": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				},
				{
						"Email": "passenger2@mailjet.com",
						"Name": "passenger 2"
				}
		]
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
```
```ruby
# This calls sends an email to the given recipient with vars and custom vars.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: "pilot@mailjet.com",
  from_name: "Mailjet Pilot",
  subject: "Your email flight plan!",
  "Mj-TemplateLanguage": true,
  text_part: "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
  html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
  vars: {'day' => 'Monday'},
  recipients: [
    {'Email' => 'passenger1@mailjet.com', 'Name' => 'passenger 1',
      'Vars' => {'day' => 'Tuesday', 'personalmessage' => 'Happy birthday!'}},
    {'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}]
)
```
```python
"""
This calls sends an email to the given recipient with vars and custom vars.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'MJ-TemplateLanguage': 'true',
  'Text-part': 'Dear passenger, welcome to Mailjet! On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}',
  'Vars': {
				"day": "Monday"
		},
  'Recipients': [
				{
						"Email": "passenger1@mailjet.com",
						"Name": "passenger 1",
						"Vars": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				},
				{
						"Email": "passenger2@mailjet.com",
						"Name": "passenger 2"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This calls sends an email to the given recipient with vars and custom vars.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
type  MyVarsStruct  struct {
  Day  string
}
type MyRecipientsVarsStruct struct {
  Day               string
  Personalmessage   string
}
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      MjTemplateLanguage: "true",
    TextPart: "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
    HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
      Vars: MyVarsStruct {
        Day: "Monday",
      },
      Recipients: []Recipient {
        Recipient {
          Email: "passenger1@mailjet.com",
          Name: "passenger 1",
          Vars: MyRecipientsVarsStruct {
            Day: "Tuesday",
            Personalmessage: "Happy birthday!",
          },
        },
        Recipient {
          Email: "passenger2@mailjet.com",
          Name: "passenger 2",
        },
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to the given recipient with vars and custom vars.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
            .Property(Send.Vars, new JObject {
                {"day", "Monday"}
                })
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger1@mailjet.com"},
                 {"Name", "passenger 1"},
                 {"Vars", new JObject {
                  {"day", "Tuesday"},
                  {"personalmessage", "Happy birthday!"}
                  }}
                 },
                new JObject {
                 {"Email", "passenger2@mailjet.com"},
                 {"Name", "passenger 2"}
                 }
                })
            .Property(Send.MjTemplateLanguage, "True");
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


Mailjet Send API allow to leverage template language in your transactional messages. 

The Mailjet Template Language include fonctionality for :

 - variable substitution
 - conditions 
 - loops
 - and a lot more... 

<aside class="notice">
The <code>Mj-TemplateLanguage</code> property in the payload provided to send API is optional but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

Find our dedicated guide [here](/template-language/).

##Adding Email Headers 

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to one recipient with a Reply-To address.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.HEADERS, new JSONObject()
                .put("Reply-To", "copilot@mailjet.com"));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
	"FromEmail":"pilot@mailjet.com",
	"FromName":"Mailjet Pilot",
	"Subject":"Your email flight plan!",
	"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
	"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
	"Recipients":[{"Email":"passenger@mailjet.com"}],
	"Headers":  {"Reply-To":"copilot@mailjet.com"}
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Headers' => [
        'Reply-To' => "copilot@mailjet.com"
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: 'pilot@mailjet.com',
  from_name: 'Mailjet Pilot',
  subject: 'Your email flight plan!',
  text_part: 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  html_part: '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  headers: {'Reply-To' => 'copilot@mailjet.com'},
  recipients:[{'Email' => 'passenger@mailjet.com'}])
```
```bash
# This calls sends an email to one recipient with a Reply-To address.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Headers" : {"Reply-To":"copilot@mailjet.com"}
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient with a Reply-To address.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Headers": {"Reply-To":"copilot@mailjet.com"}
	})
request
	.then(result => {
		console.log(result.body)
	})
	.catch(error => {
		console.log(error.statusCode)
	})
```
``` go
/*
This calls sends an email to one recipient with a Reply-To address.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      Headers: map[string]string {
        "ReplyTo": "copilot@mailjet.com",
      },
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to one recipient with a Reply-To address
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.Headers, new JObject {
                {"Reply-To", "copilot@mailjet.com"}
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


In every message, you can specify your own Email headers using the <code>Headers</code> property. For example, it is possible to specify a <code>Reply-To</code> email address.  

##Tagging Email Messages

Mailjet provides 2 properties to tag messages with your own custom information.

These custom pieces of information are returned back in the events you registered to via our [Event API](#events) and in the messages processed via our [Parse API](#customid-and-payload).

<div></div>
###Sending an email with a custom ID

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends a message to one recipient with a CustomID.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.MJCUSTOMID, "PassengerEticket1234");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
	"FromEmail":"pilot@mailjet.com",
	"FromName":"Mailjet Pilot",
	"Subject":"Your email flight plan!",
	"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
	"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
	"Recipients":[{"Email":"passenger@mailjet.com"}],
	"Mj-CustomID":"PassengerEticket1234"
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [['Email' => "passenger@mailjet.com"]],
    'Mj-CustomID' => "PassengerEticket1234"
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: 'pilot@mailjet.com',
  from_name: 'Mailjet Pilot',
  subject: 'Your email flight plan!',
  text_part: 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  html_part: '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  recipients:[{'Email' => 'passenger@mailjet.com'}],
  'Mj-CustomID' => 'PassengerEticket1234')
```
```bash
# This calls sends a message to one recipient with a CustomID.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-CustomID":"PassengerEticket1234"
	}'
```
```javascript
/**
 *
 * This calls sends a message to one recipient with a CustomID.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-CustomID":"PassengerEticket1234"
	})
request
	.then(result => {
		console.log(result.body)
	})
  .catch(error => {
    console.log(error.statusCode)
	})
```
``` go
/*
This calls sends a message to one recipient with a CustomID.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      MjCustomID: "PassengerEticket1234",
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends a message to one recipient with a CustomID
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.MjCustomID, "PassengerEticket1234");
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


Sometimes you need to use your own ID in addition to ours to be able to trace back the message in our system easily. For this purpose we let you insert your own ID in the message. To achieve this, just pass the ID you want to use in the <code>Mj-CustomID</code> property.

<div></div>
```php
<?php
/*
View : API Key Statistical campaign/message data.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'CustomID' => 'PassengerEticket1234'
];
$response = $mj->get(Resources::$Messagesentstatistics, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : API Key Statistical campaign/message data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messagesentstatistics?CustomID=PassengerEticket1234 
```
```javascript
/**
 *
 * View : API Key Statistical campaign/message data.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("messagesentstatistics")
	.request({
		"CustomID":"PassengerEticket1234"
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
# View : API Key Statistical campaign/message data.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Messagesentstatistics.all(custom_id: "PassengerEticket1234")
```
```python
"""
View : API Key Statistical campaign/message data.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'CustomID': 'PassengerEticket1234'
}
result = mailjet.messagesentstatistics.get(filters=filters)
print result.status_code
print result.json()
```
``` go
/*
View : API Key Statistical campaign/message data.
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
	var data []resources.Messagesentstatistics
	_, _, err := mailjetClient.List("messagesentstatistics", &data, Filter("CustomID", "PassengerEticket1234"))
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
import com.mailjet.client.resource.Messagesentstatistics;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : API Key Statistical campaign/message data.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Messagesentstatistics.resource)
                  .filter(Messagesentstatistics.CUSTOMID, "PassengerEticket1234");
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
      /// View : API Key Statistical campaign/message data.
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
            Resource = Messagesentstatistics.Resource,
         }
         .Filter(Messagesentstatistics.Customid, "PassengerEticket1234");
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


From then, your <code>CustomID</code> is linked to our own Message ID. You can also retrieve the message later by providing it to the <code>/messagesentstatistics</code> resource <code>CustomID</code> filter.

<div></div>
###Sending an email with a payload

```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends a message to one recipient with an EventPayload.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.MJEVENTPAYLOAD, "Eticket,1234,row,15,seat,B");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` python
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
	"FromEmail":"pilot@mailjet.com",
	"FromName":"Mailjet Pilot",
	"Subject":"Your email flight plan!",
	"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
	"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
	"Recipients":[{"Email":"passenger@mailjet.com"}],
	"Mj-EventPayLoad":"Eticket,1234,row,15,seat,B"
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```php
<?php
/*
This calls sends a message to one recipient with an EventPayload.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [['Email' => "passenger@mailjet.com"]],
    'Mj-EventPayLoad' => "Eticket,1234,row,15,seat,B"
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
  from_email: 'pilot@mailjet.com',
  from_name: 'Mailjet Pilot',
  subject: 'Your email flight plan!',
  text_part: 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  html_part: '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  recipients:[{'Email' => 'passenger@mailjet.com'}],
  'Mj-EventPayLoad' => 'Eticket,1234,row,15,seat,B')
```
```bash
# This calls sends a message to one recipient with an EventPayload.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-EventPayLoad":"Eticket,1234,row,15,seat,B"
	}'
```
```javascript
/**
 *
 * This calls sends a message to one recipient with an EventPayload.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-EventPayLoad":"Eticket,1234,row,15,seat,B"
	})
request
  .then(result => {
		console.log(result.body)
	})
  .catch(error => {
    console.log(error.statusCode)
	})
```
``` go
/*
This calls sends a message to one recipient with an EventPayload.
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      MjEventPayLoad: "Eticket,1234,row,15,seat,B",
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends a message to one recipient with an EventPayload.
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.MjEventPayload, "Eticket,1234,row,15,seat,B");
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


Sometimes, you need more than just an ID to represent the context to what a specific message is attached to. For this purpose, we let you insert a payload in the message which can be of any format (XML, JSON, CSV, etc). To take advantage of this, just pass the payload you want in the <code>Mj-EventPayLoad</code> property.

<div id="regrouping-messages-into-a-campaign"></div>
##Grouping into a campaign

``` python
"""
This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'FromEmail': 'pilot@mailjet.com',
  'FromName': 'Mailjet Pilot',
  'Subject': 'Your email flight plan!',
  'Text-part': 'Dear passenger, welcome to Mailjet! May the delivery force be with you!',
  'Html-part': '<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!',
  'Recipients': [{ Email": "passenger@mailjet.com" }],
  'Mj-campaign': 'SendAPI_campaign',
  'Mj-deduplicatecampaign': '1'
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package MyClass;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Email;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Email.resource)
						.property(Email.FROMEMAIL, "pilot@mailjet.com")
						.property(Email.FROMNAME, "Mailjet Pilot")
						.property(Email.SUBJECT, "Your email flight plan!")
						.property(Email.TEXTPART, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.MJCAMPAIGN, "SendAPI_campaign")
						.property(Email.MJDEDUPLICATECAMPAIGN, "1");
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Mj-campaign' => "SendAPI_campaign",
    'Mj-deduplicatecampaign' => 1
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
# This calls sends an email to the given recipient.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Send.create(
		from_email: "pilot@mailjet.com",
		from_name: "Mailjet Pilot",
		subject: "Your email flight plan!",
		text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		html_part: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
		'Mj-campaign' => 'SendAPI_campaign',
		'Mj-duplicatecampaign' => 1)
```
```bash
# This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[
				{
						"Email": "passenger@mailjet.com"
				}
		],
		"Mj-campaign":"SendAPI_campaign",
		"Mj-deduplicatecampaign":1
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("send")
	.request({
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
		"Html-part":"<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-campaign":"SendAPI_campaign",
		"Mj-deduplicatecampaign":1
	});
request
	.then(result => {
		console.log(result.body)
	})
	.catch(err => {
		console.log(err.statusCode)
	})
```
``` go
/*
This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
*/
package main
import (
	"fmt"
	. "github.com/mailjet/mailjet-apiv3-go"
	"os"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	email := &InfoSendMail {
      FromEmail: "pilot@mailjet.com",
      FromName: "Mailjet Pilot",
      Subject: "Your email flight plan!",
      TextPart: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      MjCampaign: "SendAPI_campaign",
      MjDeduplicateCampaign: 1,
    }
	res, err := mailjetClient.SendMail(email)
	if err != nil {
			fmt.Println(err)
	} else {
			fmt.Println("Success")
			fmt.Println(res)
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
      /// This calls sends an email to one recipient within a campaign blocking multiple email to same recipient
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
            Resource = Send.Resource,
         }
            .Property(Send.FromEmail, "pilot@mailjet.com")
            .Property(Send.FromName, "Mailjet Pilot")
            .Property(Send.Subject, "Your email flight plan!")
            .Property(Send.TextPart, "Dear passenger, welcome to Mailjet! May the delivery force be with you!")
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to Mailjet!</h3><br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.Mjcampaign, "SendAPI_campaign")
            .Property(Send.Mjdeduplicatecampaign, "1");
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


Messages sent through Send API can be regrouped into campaigns to simulate the behavior of a regular Marketing Campaign. This is useful when your transactional needs are advanced (statistics...).

Use the Property <code>Mj-campaign</code> to specify the name of the campaign the message will be classified in. If the campaign doesn't already exist it will be automatically created in Mailjet system.

By default, Mailjet lets you send multiple emails with the same campaign to the same contact. To block this feature, use <code>Mj-deduplicatecampaign</code> with the value <code>1</code> to stop contacts from being emailed several times in the same campaign.


##Send API json properties

Property Name | Description 
  ----------|------------
FromEmail | Must be a valid, activated and registered sender for this account <br />May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />**MANDATORY - MAX FROM: 1**
FromName | Must be a valid, activated and registered sender for this account <br />May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />
Sender | This can be set only on given API Keys. Contact the <a href="https://app.mailjet.com/support/ticket" target="_blank">support team</a> if you want us to enable this setting on your account.<br />Must be a valid active sender for this account.<br />Perform a simple GET on resource <code>/sender</code> to view a list of allowed senders for your account, or within the Mailjet Account Settings under <a href="https://app.mailjet.com/account/sender" target="_blank">Sender Addresses</a><br />**MAX SENDER: 1**
Recipients | List of recipients, Must include at least a property <code>Email</code> in each element<br />Sample: <code>[{"Email":"passenger@mailjet.com","Name":"passenger"}]</code><br />**MANDATORY**
To | May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />If a recipient is specified twice (in the to, cc, or bcc), it is counted only once.<br />Can be a magic list @lists.mailjet.com. See the <code>Address</code> [contactslist](/email-api/v3/contactslist/) property.<br />**MAX RECIPIENTS: 50**
Cc, Bcc | May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />If one recipient is specified twice, count as one only (including to, cc, bcc)<br />**MAX RECIPIENTS: 50**<br /> **Cc and Bcc can't be used in conjunction with Recipients property**
Subject | Maximum length is 255 chars <br />**MAX SUBJECTS: 1**
Text-part | Provides the Text part of the message<br />Mandatory if the HTML param is not specified<br />**MANDATORY IF NO HTML - MAX PARTS: 1**
Html-part | Provides the HTML part of the message<br />Mandatory if the text param is not specified<br />**MANDATORY IF NO TEXT - MAX PARTS: 1**
Mj-TemplateID | The Template ID or Name to use as this email content. Overrides the HTML/Text parts if any.<br />Equivalent to using the X-MJ-TemplateID header through SMTP.<br />**MANDATORY IF NO HTML/TEXT - MAX TEMPLATEID: 1**
Mj-TemplateLanguage | Activate the template language processing. By default the template language processing is deactivated. Use <code>True</code> to activate.<br />Equivalent to using the X-MJ-TemplateLanguage header through SMTP.<br />[More information](#use-the-template-in-send-api)
MJ-TemplateErrorReporting | Email Address where a carbon copy with error message is sent to.<br />Equivalent to using the X-MJ-TemplateErrorReporting header through SMTP.<br />[More information](../template-language/sendapi/#templates-error-management)
MJ-TemplateErrorDeliver | Define if the message is delivered if an error is discovered in the templating language. By default the delivery is deactivated. Use <code>deliver</code> to let the message be delivered to the recipient, <code>0</code> to stop it. <br />Equivalent to using the X-MJ-TemplateErrorDeliver header through SMTP.<br />[More information](../template-language/sendapi/#templates-error-management)
Attachments | Attach files automatically to this Email<br />Sum of all attachments, including inline may not exceed 15 MB total<br />Sample: [{"Content-type": "MIME TYPE", "Filename": "FILENAME.EXT", "content":"BASE64 ENCODED CONTENT"}] 
Inline_attachments | Attach a file for inline use via <code>cid:FILENAME.EXT</code><br />Sum of all attachements, including inline may not exceed 15 MB total<br />Sample: [{"Content-type": "MIME TYPE", "Filename": "FILENAME.EXT", "content":"BASE64 ENCODED CONTENT"}]
Mj-prio | Manage message processing priority inside your account (API key) scheduling queue.<br />Default is <code>2</code> as in the SMTP submission.<br />Equivalent of using <code>X-Mailjet-Prio</code> header through SMTP<br /><a href="https://app.mailjet.com/docs/email-priority-management" target="_blank">More information</a>
Mj-campaign | Groups multiple messages in one campaign<br />Equivalent of using <code>X-Mailjet-Campaign</code> header through SMTP.<br /><a href="https://app.mailjet.com/docs/emails_headers" target="_blank">More information</a>
<span style="white-space: nowrap;">Mj-deduplicatecampaign</span> | Block/unblock messages to be sent multiple times inside one campaign to the same contact.<br>- <code>0</code>: unblocked (default behavior)<br />- <code>1</code>: blocked<br />Equivalent of using <code>X-Mailjet-DeduplicateCampaign</code> header through SMTP.<br />Can only be used if mj-campaign is specified.<br /><a href="#custom-headers" target="_blank">More information</a>
Mj-trackopen | Force or disable open tracking on this message, can overriding account preferences.<br />Can only be used with a HTML part.<br />- <code>0</code>: take the values defined on the account. The ones shown <a href="https://app.mailjet.com/account/settings" target="_blank">here</a><br />- <code>1</code>: disable the tracking <br />- <code>2</code>: enable the tracking<br />A <code>X-Mailjet-TrackOpen</code> SMTP header is also available to modify the tracking behaviour.<br />[More information on custom SMTP headers](#custom-headers)
Mj-trackclick | Force or disable click tracking on this message, can overriding account preferences.<br />Can only be specified if the HTML part is provided.<br />- <code>0</code>: take the values defined on the account. The ones shown <a href="https://app.mailjet.com/account/settings" target="_blank">here</a><br />- <code>1</code>: disable the tracking <br />- <code>2</code>: enable the tracking<br />A <code>X-Mailjet-TrackClick</code> SMTP header is also available to modify the tracking behaviour.<br />[More information on custom SMTP headers](#custom-headers)
Mj-CustomID | Attach a custom ID to the message<br />Equivalent to using the X-MJ-CustomID header through SMTP.<br />[More information](#sending-an-email-with-a-custom-id)
Mj-EventPayLoad | Attach a payload to the message<br />Equivalent to using the X-MJ-EventPayload header through SMTP.<br />[More information](#sending-an-email-with-a-payload)
Headers | Add lines to the email's headers<br />List of headers as <code>"property":"value"</code> pairs<br />Notice: overriding values of header properties (ie: subject)<br />Sample: <code>{"X-My-Header":"my own value","X-My-Header-2":"my own value 2"}</code>
Vars | Global variables used for personalisation<br />Equivalent of using <code>X-MJ-Vars</code> header through SMTP.
Messages | List of messages<br /> Used for bulk emailing.<br />[More information](#sending-in-bulk)

##Send API errors

Error Code | Message | Reason 
  ----------|------------|------------
400 | Invalid characters detected | A non UTF-8 character was detected
400 | "Headers" is not JSON object type | The Headers property should follow the format {"X-My-Header":"my own value","X-My-Header-2":"my own value 2"}
400 | Header [Property name] is not string type | 
400 | Missing "To" or "Recipients" property | Send API must be called with either "To" or "Recipient" properties 
400 | Too many recipients in To <br /> Too many recipients in Cc <br /> Too many recipients in Bcc <br />|  To, Cc and Bcc can't exceed 50 Email addresses
