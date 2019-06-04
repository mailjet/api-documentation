# Send transactional email

## Choose sending method

Mailjet offers two ways to send transactional emails : SMTP Relay or Send API.

### SMTP Relay

With the Mailjet <a href="https://www.mailjet.com/feature/smtp-relay/" target="_blank">SMTP Relay</a> you can send emails in an easy way, requiring minimum integration effort on your side.

The SMTP Relay is useful if you have an existing solution for transactional emailing by SMTP or if you cannot use the Send API. Using the SMTP relay, you have to take care of email headers, MIME type handling and completely format and personalize your message content.

The best and fastest way to use the SMTP Relay is to have your own local mail server relaying messages to the Mailjet SMTP. Your local mail server will give you reliable management of the messages and connections between our 2 systems.
In case of breakage in the connection, your mail server will properly handle the error and retry sending your messages.

In case you don't have a local mail server, you can still use the SMTP Relay by using one of the many SMTP libraries available or configuring your exiting system (frameworks, CMS, CRM...). However, some of these libraries or systems can lack the advanced error handling necessary to queue and resend the messages in case of an error. The use of Send API can be a better choice as it requires less interactions between our systems and limits the risk of failures. The error handling is also a lot simpler with the API as we are managing the delivery and queuing of your messages for you.

Using Mailjet's SMTP servers to send your transactional emails is very simple. All you have to do is update your SMTP server settings to use our server as a "relay" or "smarthost" with the credentials provided by Mailjet. The credentials are your $MJ_APIKEY_PUBLIC as a login and $MJ_APIKEY_PRIVATE as a password.

You can find your SMTP credentials in your <a href="https://app.mailjet.com/account/setup" target="_blank">Account Setup page</a>

You can find more information on how to configure the SMTP Relay and custom email headers in our [SMTP Relay Guide](#SMTP_Relay_Use).


### Send API

Mailjet's Send API allows you to send transactional emails using our HTTP API, using POST requests. This solution is aimed at users needing a programmatical way to send messages.

Send API allows to send single messages but also to mutualize the calls by leveraging templating and personalization of the content.

The API will return a simple response indicating if the message is ready to be processed on the Mailjet system. This makes the error management on your side simple and efficient.

#### Choose Send API V3.1
[Send API v3.1](#send-api-v3-1) has been designed to be the most intuitive and talkative version of our transactional send APIs. It brings a human-centered payload understanding and improved error reporting.

#### Choose Send API V3
[Send API v3](#send-api-v3) is Mailjet’s traditional transactional Send API. Designed for advanced developers looking for performance and scalability, we mainly advise this version to larger players.

## Requirements

### Verify a Sender

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
```shell
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
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Sender.create(email: "anothersender@example.com"
)
p variable.attributes['Data']
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
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


To create a sender, provide the email address of the sender as part of a <code>POST</code> on the resource <code>[/sender](/reference/email/sender-addresses-and-domains/sender/)</code>.

A verification email will be sent to the address you added to activate this new sender.

<aside class="notice">The validation of a sender address can also be initiated with API calls. Visit the <a href="#domains-and-dns">Domains and DNS</a> guide for more information.</aside>

### Setup SPF/DKIM on DNS

To increase the deliverability of your emails, don't forget to properly setup your DNS record.

You can either visit [the Mailjet user interface](#verify-your-domain), or use the [Domains and DNS](#domains-and-dns) API guide to set up SPF and DKIM.


# Send API v3.1

Send API v3.1 has been released on August 2017 bringing new functionalities, better error reporting and top-notch developer experience.
Mailjet is still supporting Send API V3.

If you are a current user of Send API in version 3, we listed the [changes](#send-api-v3-to-v3-1) you will need to take into account to migrate to v3.1.

Read [this blogpost](https://www.mailjet.com/blog/news/meet-our-brand-new-send-api/) to learn more on how we designed our Send API V3.1 and where our Transactional Suite is going to.

<div id="sending-a-basic-email"></div>

## Send a basic email

```php
<?php
/*
This call sends a message to one recipient.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
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
# This call sends a message to one recipient.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!")));
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
      /// This call sends a message to one recipient.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


The Send API v3.1 sends a collection of messages, added in JSON array, called <code>Messages</code>. The input payload must start with <code>Messages</code> array. The mandatory properties for any message element are:

 - <code>From</code>: JSON object, containing 2 properties: <code>Name</code> and <code>Email</code> address of a previously [validated and active sender](https://app.mailjet.com/account/sender). Including the `Name` property in the JSON is optional. This property is not mandatory in case you use <code>TemplateID</code> and you specified a <code>From</code> address for the template. Format : <code>{ "Email":"value", "Name":"value" }</code>.
 - <code>To</code>: array of JSON objects describing each recipient. Format : <code>[{ "Email":"value", "Name":"value" },...]</code>. Here again the inclusion of the `Name` property in the JSON is optional. The same is also valid for the `Cc` and `Bcc` objects, who have the same structure.

One of the following content parts is also mandatory :

 - <code>TextPart</code> and/or <code>HtmlPart</code>: content of the message, sent in Text and/or HTML format. At least one of these content types needs to be specified. When the HTML part is the only part provided, Mailjet will not generate a Text-part from the HTML version. The property can't be set when you use <code>TemplateID</code>.
 - <code>TemplateID</code>: an ID for a template that is previously created and stored in Mailjet's system. It is mandatory when <code>From</code> and <code>TextPart</code> and/or <code>HtmlPart</code> are not provided. Visit the [Use a Template](#using-a-template) section for more information.

<aside class="notice">
Important: The recipients listed in <code>To</code> will receive a common message, showing every other recipient and carbon copy (CC) recipients. If you do not wish the recipients to see each other, you have to create multiple messages in the <code>Messages</code> array.
<div></div>
</aside>

> API response

```json

{
  "Messages": [
    {
      "Status": "success",
      "To": [
        {
        "Email": "passenger1@mailjet.com",
        "MessageUUID": "123",
        "MessageID": 456,
        "MessageHref": "https://api.mailjet.com/v3/message/456"
        }
      ]
     }
  ]
}
```

Send API will send a response containing an array of <code>Messages</code>. Each instance of the message object will include the <code>Status</code> and the list of message UUIDs for each recipient in <code>To</code>, <code>Cc</code> and <code>Bcc</code>.


<code>MessageUUID</code> is the internal Mailjet ID of your message.

<code>MessageID</code> is the unique ID of the message (legacy format). You will be able to use this ID to get more information about your message.

<code>MessageHref</code> is a URL, pointing to the API URL, where the message metadata can be retrieved. It is made of the API Base URL, the message resource path and the message ID (not UUID).



<aside class="notice">
NOTICE: If you send an email to a contact, which is not registered in Mailjet, the system will automatically create and save it. Keep this in mind if you intend to use this email address later (for example to add it to a contact list), as it will already exist in Mailjet and there's no need to create it again.
</aside>

<div></div>

```php
<?php
/*
This call sends a message to one recipient.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ],
                [
                    'Email' => "passenger2@mailjet.com",
                    'Name' => "passenger 2"
                ]
            ],
            'Cc' => [
                [
                    'Email' => "copilot@mailjet.com",
                    'Name' => "Copilot"
                ]
            ],
            'Bcc' => [
                [
                    'Email' => "air-traffic-control@mailjet.com",
                    'Name' => "Air traffic control"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								},
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Cc": [
								{
										"Email": "copilot@mailjet.com",
										"Name": "Copilot"
								}
						],
						"Bcc": [
								{
										"Email": "air-traffic-control@mailjet.com",
										"Name": "Air traffic control"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								},
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Cc": [
								{
										"Email": "copilot@mailjet.com",
										"Name": "Copilot"
								}
						],
						"Bcc": [
								{
										"Email": "air-traffic-control@mailjet.com",
										"Name": "Air traffic control"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
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
# This call sends a message to one recipient.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        },
        {
            'Email'=> 'passenger2@mailjet.com',
            'Name'=> 'passenger 2'
        }
    ],
    'Cc'=> [
        {
            'Email'=> 'copilot@mailjet.com',
            'Name'=> 'Copilot'
        }
    ],
    'Bcc'=> [
        {
            'Email'=> 'air-traffic-control@mailjet.com',
            'Name'=> 'Air traffic control'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								},
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Cc": [
								{
										"Email": "copilot@mailjet.com",
										"Name": "Copilot"
								}
						],
						"Bcc": [
								{
										"Email": "air-traffic-control@mailjet.com",
										"Name": "Air traffic control"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
          mailjet.RecipientV31 {
            Email: "passenger2@mailjet.com",
            Name: "passenger 2",
          },
        },
        Cc: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "copilot@mailjet.com",
            Name: "Copilot",
          },
        },
        Bcc: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "air-traffic-control@mailjet.com",
            Name: "Air traffic control",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1"))
                        .put(new JSONObject()
                            .put("Email", "passenger2@mailjet.com")
                            .put("Name", "passenger 2")))
                    .put(Emailv31.Message.CC, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "copilot@mailjet.com")
                            .put("Name", "Copilot")))
                    .put(Emailv31.Message.BCC, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "air-traffic-control@mailjet.com")
                            .put("Name", "Air traffic control")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!")));
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
      /// This call sends a message to one recipient.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   },
                  new JObject {
                   {"Email", "passenger2@mailjet.com"},
                   {"Name", "passenger 2"}
                   }
                  }},
                 {"Cc", new JArray {
                  new JObject {
                   {"Email", "copilot@mailjet.com"},
                   {"Name", "Copilot"}
                   }
                  }},
                 {"Bcc", new JArray {
                  new JObject {
                   {"Email", "air-traffic-control@mailjet.com"},
                   {"Name", "Air traffic control"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


> API response

```json
{
  "Messages": [
    {
    "Status": "success",
    "To": [
        {
        "Email": "passenger1@mailjet.com",
        "MessageUUID": "123",
        "MessageID": 456,
        "MessageHref": "https://api.mailjet.com/v3/message/456"
        },
        {
        "Email": "passenger2@mailjet.com",
        "MessageUUID": "124",
        "MessageID": 457,
        "MessageHref": "https://api.mailjet.com/v3/message/457"
        }
      ],
    "Cc": [
        {
        "Email": "copilot@mailjet.com",
        "MessageUUID": "125",
        "MessageID": 458,
        "MessageHref": "https://api.mailjet.com/v3/message/458"
        }
      ],
    "Bcc": [
        {
        "Email": "air-traffic-control@mailjet.com",
        "MessageUUID": "126",
        "MessageID": 459,
        "MessageHref": "https://api.mailjet.com/v3/message/459"
        }
      ]
    }
  ]
}
```

<div></div>


<div id="sending-with-attached-files"></div>

## Send with attached files

```php
<?php
/*
This call sends a message to the given recipient with attachment.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
            'Attachments' => [
                [
                    'ContentType' => "text/plain",
                    'Filename' => "test.txt",
                    'Base64Content' => "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
                ]
            ]
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to the given recipient with attachment.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"Attachments": [
								{
										"ContentType": "text/plain",
										"Filename": "test.txt",
										"Base64Content": "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
								}
						]
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to the given recipient with attachment.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"Attachments": [
								{
										"ContentType": "text/plain",
										"Filename": "test.txt",
										"Base64Content": "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
								}
						]
				}
		]
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
# This call sends a message to the given recipient with attachment.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!',
    'Attachments'=> [
        {
            'ContentType'=> 'text/plain',
            'Filename'=> 'test.txt',
            'Base64Content'=> 'VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK'
        }
    ]
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to the given recipient with attachment.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"Attachments": [
								{
										"ContentType": "text/plain",
										"Filename": "test.txt",
										"Base64Content": "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"
								}
						]
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to the given recipient with attachment.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
        Attachments: &mailjet.AttachmentsV31{
          mailjet.AttachmentV31 {
            ContentType: "text/plain",
            Filename: "test.txt",
            Base64Content: "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK",
          },
        },
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to the given recipient with attachment.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!")
                    .put(Emailv31.Message.ATTACHMENTS, new JSONArray()
                        .put(new JSONObject()
                            .put("ContentType", "text/plain")
                            .put("Filename", "test.txt")
                            .put("Base64Content", "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK")))));
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
      /// This call sends a message to the given recipient with attachment.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"},
                 {"Attachments", new JArray {
                  new JObject {
                   {"ContentType", "text/plain"},
                   {"Filename", "test.txt"},
                   {"Base64Content", "VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK"}
                   }
                  }}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


To attach files, use the properties <code>Attachments</code> or <code>InlinedAttachments</code>.  
When using <code>Attachments</code>, the attachment will be separately added as a file and the recipient should click on it in order to see it. Normally, the inlined attachment(s) should be visible directly in the body of the message, but this depends on the recipient's email client behavior.
In both calls, the content needs to be Base64 encoded. You also need to specify the MIME type and a file name.



<div></div>

```php
<?php
/*
This call sends a message to the given recipient with inline attachment.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
            'InlinedAttachments' => [
                [
                    'ContentType' => "image/png",
                    'Filename' => "logo.png",
                    'ContentID' => "id1",
                    'Base64Content' => "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
                ]
            ]
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to the given recipient with inline attachment.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"InlinedAttachments": [
								{
										"ContentType": "image/png",
										"Filename": "logo.png",
										"ContentID": "id1",
										"Base64Content": "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
								}
						]
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to the given recipient with inline attachment.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"InlinedAttachments": [
								{
										"ContentType": "image/png",
										"Filename": "logo.png",
										"ContentID": "id1",
										"Base64Content": "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
								}
						]
				}
		]
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
# This call sends a message to the given recipient with inline attachment.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <img src=\'cid:id1\'> <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!',
    'InlinedAttachments'=> [
        {
            'ContentType'=> 'image/png',
            'Filename'=> 'logo.png',
            'ContentID'=> 'id1',
            'Base64Content'=> 'iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=='
        }
    ]
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to the given recipient with inline attachment.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"InlinedAttachments": [
								{
										"ContentType": "image/png",
										"Filename": "logo.png",
										"ContentID": "id1",
										"Base64Content": "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
								}
						]
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to the given recipient with inline attachment.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
        InlinedAttachments: &mailjet.InlinedAttachmentsV31{
          mailjet.InlineAttachmentV31 {
            ContentType: "image/png",
            Filename: "logo.png",
            ContentID: "id1",
            Base64Content: "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg==",
          },
        },
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to the given recipient with inline attachment.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!")
                    .put(Emailv31.Message.INLINEDATTACHMENTS, new JSONArray()
                        .put(new JSONObject()
                            .put("ContentType", "image/png")
                            .put("Filename", "logo.png")
                            .put("ContentID", "id1")
                            .put("Base64Content", "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg==")))));
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
      /// This call sends a message to the given recipient with inline attachment.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <img src=\"cid:id1\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"},
                 {"InlinedAttachments", new JArray {
                  new JObject {
                   {"ContentType", "image/png"},
                   {"Filename", "logo.png"},
                   {"ContentID", "id1"},
                   {"Base64Content", "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="}
                   }
                  }}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


When using an inlined attachment, it's possible to insert the file inside the HTML code of the email by using <code>cid:FILENAME.EXT</code>, where FILENAME.EXT is the <code>Filename</code> specified in the declaration of the attachment.
Optionally, you can set <code>ContentID</code>. It's converted to a <code>Content-ID</code> SMTP header. The value set must be unique - Mailjet isn't enforcing it - among all the inline attachments and can be used to reference the inlined attachment in the message body, using the following syntax in HTML (since plain text messages can not contain images): <code> &lt;img src="cid:myimagecid"/&gt;</code>

<aside class="warning">
Remember to keep the size of your attachments small. They should not exceed 15 MB.
</aside>

<div id="sending-in-bulk"></div>

## Send in bulk

```php
<?php
/*
This call sends 2 messages to 2 different recipients.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
        ],
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger2@mailjet.com",
                    'Name' => "passenger 2"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends 2 messages to 2 different recipients.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				},
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends 2 messages to 2 different recipients.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				},
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		]
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
# This call sends 2 messages to 2 different recipients.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!'
}, {
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger2@mailjet.com',
            'Name'=> 'passenger 2'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 2, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 2, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends 2 messages to 2 different recipients.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				},
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger2@mailjet.com",
										"Name": "passenger 2"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends 2 messages to 2 different recipients.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
      },
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger2@mailjet.com",
            Name: "passenger 2",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends 2 messages to 2 different recipients.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"))
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger2@mailjet.com")
                            .put("Name", "passenger 2")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")));
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
      /// This call sends 2 messages to 2 different recipients.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"}
                 },
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger2@mailjet.com"},
                   {"Name", "passenger 2"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


> API response:

```json
{
  "Messages": [
    {
    "Status": "success",
    "To": [
        {
        "Email": "passenger1@mailjet.com",
        "MessageUUID": "123",
        "MessageID": 20547681647433000,
        "MessageHref": "https://api.mailjet.com/v3/message/20547681647433000"
        }
      ]
    },
    {
    "Status": "success",
    "To": [
        {
        "Email": "passenger2@mailjet.com",
        "MessageUUID": "124",
        "MessageID": 20547681647433001,
        "MessageHref": "https://api.mailjet.com/v3/message/20547681647433001"
        }
      ]
    }
  ]
}
```
To send messages in bulk, package the multiple messages inside the <code>Messages</code> property.

The messages’ order is preserved from the user input, allowing you to identify which message response corresponds to your original message payload.

<div></div>
> API response with 1 message in error

```json
{
	"Messages": [
		{
			"Errors": [
				{
					"ErrorIdentifier": "88b5ca9f-5f1f-42e7-a45e-9ecbad0c285e",
					"ErrorCode": "send-0003",
					"StatusCode": 400,
					"ErrorMessage": "At least \"HTMLPart\", \"TextPart\" or \"TemplateID\" must be provided.",
					"ErrorRelatedTo": [
						"HTMLPart",
						"TextPart"
					]
				}
			],
			"Status": "error"
		},
		{
			"Status": "success",
			"CustomID": "",
			"To": [
				{
					"Email": "passenger2@mailjet.com",
					"MessageUUID": "cb927469-36fd-4c02-bce4-0d199929a207",
					"MessageID": 70650219165027410,
					"MessageHref": "https://api.mailjet.com/v3/REST/message/70650219165027410"
				}
			],
			"Cc": [],
			"Bcc": []
		}
	]
}
```

In case of errors on one or several of the messages, the API will not stop the processing of other successful messages. All validated messages will be processed for sending and the response will include both MessageIDs and Error reports.

<div id="personalisation"></div>
## Personalization

### Content formatting

Mailjet offers a templating language that allows you to personalize your transactional messages. It enables you to insert data in your text or HTML parts.

To do so, use <code>{{DATA_TYPE:DATA_NAME}}</code> where:

- <code>DATA_TYPE</code>: <code>var</code> for Variables specified in the API call or <code>data</code> for contact data, which is already available in the Mailjet system
- <code>DATA_NAME</code>: name of the data you want to insert


<aside class="notice">
NOTICE: The <code>TemplateLanguage</code> property should be set to <code>true</code> to force Send API to interpret the template language (<code>X-MJ-TemplateLanguage</code> in case you are using SMTP).
</aside>

Visit our [Transactional templating](../template-language) guide to learn about additional substitutions, modification functions and conditional statements you can use to personalize your messages.

<div id="using-vars-and-custom-vars"></div>

### Use vars and custom vars

```php
<?php
/*
This call sends a message to a recipient with global personalisation.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Variables' => [
                'day' => "Monday"
            ],
            'TemplateLanguage' => true,
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to a recipient with global personalisation.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Variables": {
								"day": "Monday"
						},
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to a recipient with global personalisation.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Variables": {
								"day": "Monday"
						},
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!"
				}
		]
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
# This call sends a message to a recipient with global personalisation.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Variables'=> {
        'day'=> 'Monday'
    },
    'TemplateLanguage'=> true,
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
``` go
/*
This call sends a message to a recipient with global personalisation.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
      Variables: map[string]interface{}{
      "day": "Monday"},
      TemplateLanguage: true,
      Subject: "Your email flight plan!",
    TextPart: "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!",
    HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!",
    },
  },
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```python
"""
This call sends a message to a recipient with global personalisation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Variables": {
								"day": "Monday"
						},
						"TemplateLanguage": True,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to a recipient with global personalisation.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.VARIABLES, new JSONObject()
                        .put("day", "Monday"))
                    .put(Emailv31.Message.TEMPLATELANGUAGE, true)
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!")));
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
      /// This call sends a message to a recipient with global personalisation.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Variables", new JObject {
                  {"day", "Monday"}
                  }},
                 {"TemplateLanguage", true},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger, welcome to Mailjet! On this {{var:day}}, may the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />On this {{var:day}}, may the delivery force be with you!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


By using <code>Variables</code> in conjunction with the <code>{{var:VAR_NAME}}</code> or <code>{{var:VAR_NAME:DEFAULT_VALUE}}</code> , you can modify the content of your email with variables, pushed in your Send API call.

<code>DEFAULT_VALUE</code>is the default value that will be used, if the variable is not defined in the API call.

<div id="using-contact-properties"></div>

### Use contact properties

```php
<?php
/*
This call sends a message to the a recipient with contact property personalisation.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'TemplateLanguage' => true,
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * This call sends a message to the a recipient with contact property personalisation.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!"
				}
		]
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
# This call sends a message to the a recipient with contact property personalisation.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'TemplateLanguage'=> true,
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear {{data:firstname:\'passenger\'}}, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear {{data:firstname:\'passenger\'}}, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br /> May the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to the a recipient with contact property personalisation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateLanguage": True,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to the a recipient with contact property personalisation.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        TemplateLanguage: true,
        Subject: "Your email flight plan!",
      TextPart: "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!",
      HTMLPart: "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to the a recipient with contact property personalisation.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.TEMPLATELANGUAGE, true)
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!")));
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
      /// This call sends a message to the a recipient with contact property personalisation.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"TemplateLanguage", true},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```
```shell
# This call sends a message to the a recipient with contact property personalisation.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"TextPart": "Dear {{data:firstname:\"passenger\"}}, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear {{data:firstname:\"passenger\"}}, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!"
				}
		]
	}'
```


If the contact you are sending an email to is already existing in the Mailjet system with some contact data, you can leverage this information to personalize your email.

Use <code>{{data:METADATA_NAME}}</code> or <code>{{data:METADATA_NAME:DEFAULT_VALUE}}</code> to insert data in your content.

<code>DEFAULT_VALUE</code> is the default value that will be used if no data is found.

Refer to the [Personalization section](/guides/#personalization-add-contact-properties) for more information on how to add contact properties.

<div id="using-a-template"></div>
## Use a Template

```php
<?php
/*
This call sends a message based on a template.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'TemplateID' => 1,
            'TemplateLanguage' => true,
            'Subject' => "Your email flight plan!"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message based on a template.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateID": 1,
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message based on a template.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateID": 1,
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!"
				}
		]
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
# This call sends a message based on a template.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'TemplateID'=> 1,
    'TemplateLanguage'=> true,
    'Subject'=> 'Your email flight plan!'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message based on a template.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TemplateID": 1,
						"TemplateLanguage": True,
						"Subject": "Your email flight plan!"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message based on a template.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        TemplateID: 1,
        TemplateLanguage: true,
        Subject: "Your email flight plan!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message based on a template.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.TEMPLATEID, "1")
                    .put(Emailv31.Message.TEMPLATELANGUAGE, true)
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")));
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
      /// This call sends a message based on a template.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"TemplateID", 1},
                 {"TemplateLanguage", true},
                 {"Subject", "Your email flight plan!"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Mailjet offers to store your transactional message templates on its platform. You can use these templates to avoid repeating the content of a transactional message at each Send API call.

You can either create the templates through our online drag and drop tool [Passport](https://www.mailjet.com/feature/passport/) or through the [/template](/reference/email/templates/) resource.

You can learn more about storing your templates through API [here](../guides/#storing-a-template).

You can also follow our [Step by Step guide](../template-language/passport) to create your first Passport template with templating language.

In the templates, you will be able to use simple [personalization](#personalization) (<code>[[data:property_name]]</code> or <code>[[var:variable_name]]</code>) or advanced [templating language](../template-language) (<code>{{data:property_name}}</code>, <code>{{var:variable_name}}</code>, conditional statements and loop statements)

In this sample, <code>TemplateID</code> will be the ID provided by Passport at the end of your designing process or the ID returned by the <code>/template</code> resource.

<aside class="notice">
The <code>TemplateLanguage</code> property in the payload provided to Send API is optional, but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

<div id="using-template-language"></div>

## Use Templating Language

```php
<?php
/*
This call sends a message to the given recipient with vars and custom vars.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'TextPart' => "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
            'HTMLPart' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
            'TemplateLanguage' => true,
            'Subject' => "Your email flight plan!",
            'Variables' => [
                'day' => "Tuesday",
                'personalmessage' => "Happy birthday!"
            ]
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to the given recipient with vars and custom vars.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"Variables": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to the given recipient with vars and custom vars.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"TemplateLanguage": true,
						"Subject": "Your email flight plan!",
						"Variables": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				}
		]
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
# This call sends a message to the given recipient with vars and custom vars.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'TextPart'=> 'Dear passenger, welcome to Mailjet! On this {{var:day:\'monday\'}}, may the delivery force be with you! {{var:personalmessage:\'\'}}',
    'HTMLPart'=> '<h3>Dear passenger, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br /> On this {{var:day:\'monday\'}}, may the delivery force be with you! {{var:personalmessage:\'\'}}',
    'TemplateLanguage'=> true,
    'Subject'=> 'Your email flight plan!',
    'Variables'=> {
        'day'=> 'Tuesday',
        'personalmessage'=> 'Happy birthday!'
    }
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to the given recipient with vars and custom vars.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"TextPart": "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"HTMLPart": "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
						"TemplateLanguage": True,
						"Subject": "Your email flight plan!",
						"Variables": {
								"day": "Tuesday",
								"personalmessage": "Happy birthday!"
						}
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to the given recipient with vars and custom vars.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
      TextPart: "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
        TemplateLanguage: true,
        Subject: "Your email flight plan!",
      Variables: map[string]interface{}{
      "day": "Tuesday""personalmessage": "Happy birthday!"},
    },
  },
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to the given recipient with vars and custom vars.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.TEXTPART, "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
                    .put(Emailv31.Message.TEMPLATELANGUAGE, true)
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.VARIABLES, new JSONObject()
                        .put("day", "Tuesday")
                        .put("personalmessage", "Happy birthday!"))));
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
      /// This call sends a message to the given recipient with vars and custom vars.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"TextPart", "Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}"},
                 {"HTMLPart", "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}"},
                 {"TemplateLanguage", true},
                 {"Subject", "Your email flight plan!"},
                 {"Variables", new JObject {
                  {"day", "Tuesday"},
                  {"personalmessage", "Happy birthday!"}
                  }}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Mailjet Send API allows you to leverage the Mailjet templating language in your transactional messages.

The Mailjet Templating Language enables you to achieve:

 - variable substitution
 - conditions, including usage of contacts segments
 - loops
 - and a lot more...

<aside class="notice">
The <code>TemplateLanguage</code> property in the payload provided to Send API is optional but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

Find our dedicated guide [here](/template-language/).

<div id="adding-email-headers"></div>

## Add Email Headers

```php
<?php
/*
This call sends an email to one recipient with an additional SMTP header
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
            'Headers' => [
                'X-My-header' => "X2332X-324-432-534"
            ]
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends an email to one recipient with an additional SMTP header
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"Headers": {
								"X-My-header": "X2332X-324-432-534"
						}
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends an email to one recipient with an additional SMTP header
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"Headers": {
								"X-My-header": "X2332X-324-432-534"
						}
				}
		]
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
# This call sends an email to one recipient with an additional SMTP header
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!',
    'Headers'=> {
        'X-My-header'=> 'X2332X-324-432-534'
    }
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends an email to one recipient with an additional SMTP header
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"Headers": {
								"X-My-header": "X2332X-324-432-534"
						}
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends an email to one recipient with an additional SMTP header
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
                    .put(Emailv31.Message.HEADERS, new JSONObject()
                        .put("X-My-header", "X2332X-324-432-534"))));
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
      /// This call sends an email to one recipient with an additional SMTP header
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"},
                 {"Headers", new JObject {
                  {"X-My-header", "X2332X-324-432-534"}
                  }}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```
``` go
/*
This call sends an email to one recipient with an additional SMTP header
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
      Headers: map[string]interface{}{
      "X-My-header": "X2332X-324-432-534"},
    },
  },
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```


In every message, you can specify your own Email headers using the <code>Headers</code> property. These headers will be added to the SMTP headers of the message, delivered to the recipient.  

Only headers that don’t have a dedicated property in the message payload can be customized through the <code>Headers</code> property.
In addition, some headers can’t be customized.

List of forbidden headers :

|  |  |  |  |  |
|---|---|---|---|---|
|  | From | Sender | Subject | To |
|  | Cc | Bcc | Return-Path | Delivered-To |
|  | DKIM-Signature | DomainKey-Status | Received-SPF | Authentication-Results |
|  | Received | X-Mailjet-Prio | X-Mailjet-Debug | User-Agent |
|  | X-Mailer | X-MJ-CustomID | X-MJ-EventPayload | X-MJ-Vars |
|  | X-MJ-TemplateErrorDeliver | X-MJ-TemplateErrorReporting | X-MJ-TemplateLanguage | X-Mailjet-TrackOpen |
|  | X-Mailjet-TrackClick | X-MJ-TemplateID | X-MJ-WorkflowID | X-Feedback-Id |
|  | X-Mailjet-Segmentation | List-Id | X-MJ-MID | X-MJ-ErrorMessage |
|  | Date | X-CSA-Complaints | Message-Id | X-Mailjet-Campaign |
|  | X-MJ-StatisticsContactsListID | |  |

<div id="tagging-email-messages"></div>

## Tag Email Messages

Mailjet provides 2 properties to tag messages with your own custom information.

These custom tags are included in the events triggered by our [Event API](#events) and in the messages processed via our [Parse API](#customid-and-payload).

<div id="sending-an-email-with-a-custom-id"></div>

### Send an email with a custom ID

```php
<?php
/*
This call sends a message to one recipient with a CustomID
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
            'CustomID' => "PassengerEticket1234"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient with a CustomID
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"CustomID": "PassengerEticket1234"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient with a CustomID
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"CustomID": "PassengerEticket1234"
				}
		]
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
# This call sends a message to one recipient with a CustomID
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!',
    'CustomID'=> 'PassengerEticket1234'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient with a CustomID
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"CustomID": "PassengerEticket1234"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient with a CustomID
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
        CustomID: "PassengerEticket1234",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient with a CustomID
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
                    .put(Emailv31.Message.CUSTOMID, "PassengerEticket1234")));
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
      /// This call sends a message to one recipient with a CustomID
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"},
                 {"CustomID", "PassengerEticket1234"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Sometimes you may need to use your own ID, in addition to ours, to be able to easily trace back the message in our system. To achieve this, just pass the ID you wish in the <code>CustomID</code> property.

<div></div>
```shell
# View : API Key Statistical campaign/message data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message?CustomID=PassengerEticket1234 
```
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
$response = $mj->get(Resources::$Message, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
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
	.get("message")
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
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Message.all(custom_id: "PassengerEticket1234"
)
p variable.attributes['Data']
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
result = mailjet.message.get(filters=filters)
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
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	var data []resources.Message
	_, _, err := mailjetClient.List("message", &data, Filter("CustomID", "PassengerEticket1234"))
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
import com.mailjet.client.resource.Message;
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
      request = new MailjetRequest(Message.resource)
                  .filter(Message.CUSTOMID, "PassengerEticket1234");
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
            Resource = Message.Resource,
         }
         .Filter(Message.Customid, "PassengerEticket1234");
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Your <code>CustomID</code> will be linked to our own UUID. You can also retrieve the message later by providing it to the <code>/message</code> resource with <code>CustomID</code> filter.

<div id="sending-an-email-with-a-payload"></div>

### Send an email with a payload

```php
<?php
/*
This call sends a message to one recipient with an EventPayload.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
            'EventPayload' => "Eticket,1234,row,15,seat,B"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient with an EventPayload.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"EventPayload": "Eticket,1234,row,15,seat,B"
				}
		]
	}'
```
```ruby
# This call sends a message to one recipient with an EventPayload.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!',
    'EventPayload'=> 'Eticket,1234,row,15,seat,B'
}]
)
p variable.attributes['Messages']
```
```javascript
/**
 *
 * This call sends a message to one recipient with an EventPayload.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"EventPayload": "Eticket,1234,row,15,seat,B"
				}
		]
	})
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
This call sends a message to one recipient with an EventPayload.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"EventPayload": "Eticket,1234,row,15,seat,B"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient with an EventPayload.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
        EventPayload: "Eticket,1234,row,15,seat,B",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient with an EventPayload.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
                    .put(Emailv31.Message.EVENTPAYLOAD, "Eticket,1234,row,15,seat,B")));
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
      /// This call sends a message to one recipient with an EventPayload.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"},
                 {"EventPayload", "Eticket,1234,row,15,seat,B"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Sometimes, you need more than just an ID to represent the context of a specific message. For this purpose, we let you insert a payload in the message which can be of any format (XML, JSON, CSV, etc). To take advantage of this, just pass the payload you want in the <code>EventPayload</code> property.

<div id="regrouping-messages-into-a-campaign"></div>
<div id="grouping-into-a-campaign"></div>

## Group into a campaign

```php
<?php
/*
This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
            'CustomCampaign' => "SendAPI_campaign",
            'DeduplicateCampaign' => true
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"CustomCampaign": "SendAPI_campaign",
						"DeduplicateCampaign": true
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"CustomCampaign": "SendAPI_campaign",
						"DeduplicateCampaign": true
				}
		]
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
# This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!',
    'CustomCampaign'=> 'SendAPI_campaign',
    'DeduplicateCampaign'=> true
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"CustomCampaign": "SendAPI_campaign",
						"DeduplicateCampaign": True
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!",
        CustomCampaign: "SendAPI_campaign",
        DeduplicateCampaign: true,
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!")
                    .put(Emailv31.Message.CUSTOMCAMPAIGN, "SendAPI_campaign")
                    .put(Emailv31.Message.DEDUPLICATECAMPAIGN, true)));
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
      /// This call sends a message to one recipient within a campaign blocking multiple messages to same recipient
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"},
                 {"CustomCampaign", "SendAPI_campaign"},
                 {"DeduplicateCampaign", true}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Messages sent through Send API can be regrouped into campaigns to simulate the behavior of a regular marketing campaign. This could help you with pulling advanced statistics for your transactional campaigns.

Use the Property <code>CustomCampaign</code> to specify the name of the campaign the message will be classified in. If the campaign doesn't already exist, it will be automatically created in the Mailjet system.

By default, Mailjet lets you send multiple emails with the same campaign to the same contact. To block this feature, use <code>DeduplicateCampaign</code> with the value <code>1</code> to stop contacts from being emailed several times in the same campaign.

<div id="adding-url-tags"></div>

## Add URL tags

```php
<?php
/*
This calls sends an email to one recipient.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!",
            'URLTags' => "param1=1&param2=2"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This calls sends an email to one recipient.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"URLTags": "param1=1&param2=2"
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
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"URLTags": "param1=1&param2=2"
				}
		]
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
# This calls sends an email to one recipient.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'http://www.mailjet.com\'>Mailjet</a>!</h3><br />May the delivery force be with you!',
    'URLTags'=> 'param1=1&param2=2'
}]
)
p variable.attributes['Messages']
```
```python
"""
This calls sends an email to one recipient.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!",
						"URLTags": "param1=1&param2=2"
				}
		]
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
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!",
        URLTags: "param1=1&param2=2",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
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
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!")
                    .put(Emailv31.Message.URLTAGS, "param1=1&param2=2")));
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
      /// This calls sends an email to one recipient.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"http://www.mailjet.com\">Mailjet</a>!</h3><br />May the delivery force be with you!"},
                 {"URLTags", "param1=1&param2=2"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


If you need to add tracking parameters in all your URLS in one simple way in your message, Send API offers the <code>URLTags</code> property. This solution will be perfect for passing UTM parameters for your traffic analytics in a easy way, without having to modify every single URLs in your message yourself.

You just need to provide the query part between the first "?" character and "#" character.

So if you want to have a URL in this format: <code>http://www.example.com?param1=1&param2=2</code>

You will just need to specify : <code>"URLTags":"param1=1&param2=2"</code>

In your HTMLPart or template, you will only need to specify the href <code>http://www.example.com</code>.

Mailjet will add the parameters in all of the URLs in your message, before adding the Mailjet click tracking and sending the message. The URLs in your click statistics will include the URLTags provided.

<aside class="notice">
NOTICE: The string provided needs to be properly encoded (ie : <code>space</code> becomes <code>%20</code>, <code>"</code> becomes <code>%22</code> ... ) see more information <a href="https://en.wikipedia.org/wiki/Percent-encoding" target="_blank">here</a>
</aside>

## Sandbox Mode

```php
<?php
/*
This call sends a message to one recipient in sandbox mode.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
        ]
    ],
    'SandboxMode' => true
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient in sandbox mode.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		],
		"SandboxMode":true
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient in sandbox mode.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		],
		"SandboxMode":true
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
# This call sends a message to one recipient in sandbox mode.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!'
}],
sandbox_mode: true
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient in sandbox mode.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
				}
		],
  'SandboxMode': True
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient in sandbox mode.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo, SandboxMode: true }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient in sandbox mode.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")))
			.property(Emailv31.SANDBOXMODE, true);
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
      /// This call sends a message to one recipient in sandbox mode.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"}
                 }
                })
            .Property(Send.SandboxMode, true);
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


> API response:

```json
{
  "Messages": [
    {
      "Status": "success",
      "CustomID": "",
      "To": [
        {
          "Email": "passenger1@mailjet.com",
          "MessageUUID": "",
          "MessageID": 0,
          "MessageHref": "https://api.mailjet.com/v3/message/0"
        }
      ],
      "Cc": [],
      "Bcc": []
    }
  ]
}
```

The Send API v3.1 allows to run the API call in a Sandbox mode, where all validations of the payload will be done without delivering the message.

By setting the <code>SandboxMode</code> property to a <code>true</code> value, you will turn off the delivery of the message while still getting back the full range of error messages that could be related to your message processing.
If the message is processed without error, the response will follow the normal response payload format, omitting only the <code>MessageID</code> and <code>MessageUUID</code>.

<aside class="notice">
NOTICE: The <code>SandboxMode</code> property is a Send API JSON payload root property like <code>Messages</code>, not a message JSON property.
</aside>

## Real-time Monitoring

```php
<?php
/*
This call sends a message to one recipient with Real-time Monitoring.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "pilot@mailjet.com",
                'Name' => "Mailjet Pilot"
            ],
            'To' => [
                [
                    'Email' => "passenger1@mailjet.com",
                    'Name' => "passenger 1"
                ]
            ],
            'Subject' => "Your email flight plan!",
            'TextPart' => "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
            'MonitoringCategory' => "Category1"
        ]
    ]
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# This call sends a message to one recipient with Real-time Monitoring.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"MonitoringCategory": "Category1"
				}
		]
	}'
```
```javascript
/**
 *
 * This call sends a message to one recipient with Real-time Monitoring.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("send", {'version': 'v3.1'})
	.request({
		"Messages":[
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"MonitoringCategory": "Category1"
				}
		]
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
# This call sends a message to one recipient with Real-time Monitoring.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> 'pilot@mailjet.com',
        'Name'=> 'Mailjet Pilot'
    },
    'To'=> [
        {
            'Email'=> 'passenger1@mailjet.com',
            'Name'=> 'passenger 1'
        }
    ],
    'Subject'=> 'Your email flight plan!',
    'TextPart'=> 'Dear passenger 1, welcome to Mailjet! May the delivery force be with you!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!<br />May the delivery force be with you!',
    'MonitoringCategory'=> 'Category1'
}]
)
p variable.attributes['Messages']
```
```python
"""
This call sends a message to one recipient with Real-time Monitoring.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret), version='v3.1')
data = {
  'Messages': [
				{
						"From": {
								"Email": "pilot@mailjet.com",
								"Name": "Mailjet Pilot"
						},
						"To": [
								{
										"Email": "passenger1@mailjet.com",
										"Name": "passenger 1"
								}
						],
						"Subject": "Your email flight plan!",
						"TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
						"MonitoringCategory": "Category1"
				}
		]
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This call sends a message to one recipient with Real-time Monitoring.
*/
package main
import (
	"fmt"
	"log"
	"os"
	mailjet "github.com/mailjet/mailjet-apiv3-go"
)
func main () {
	mailjetClient := NewMailjetClient(os.Getenv("MJ_APIKEY_PUBLIC"), os.Getenv("MJ_APIKEY_PRIVATE"))
	messagesInfo := []mailjet.InfoMessagesV31 {
      mailjet.InfoMessagesV31{
        From: &mailjet.RecipientV31{
          Email: "pilot@mailjet.com",
          Name: "Mailjet Pilot",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "passenger1@mailjet.com",
            Name: "passenger 1",
          },
        },
        Subject: "Your email flight plan!",
        TextPart: "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
        HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
        MonitoringCategory: "Category1",
      },
    }
	messages := mailjet.MessagesV31{Info: messagesInfo }
	res, err := m.SendMailV31(&messages)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Data: %+v\n", res)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.errors.MailjetSocketTimeoutException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.ClientOptions;
import com.mailjet.client.resource.Emailv31;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * This call sends a message to one recipient with Real-time Monitoring.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"), new ClientOptions("v3.1"));
      request = new MailjetRequest(Emailv31.resource)
			.property(Emailv31.MESSAGES, new JSONArray()
                .put(new JSONObject()
                    .put(Emailv31.Message.FROM, new JSONObject()
                        .put("Email", "pilot@mailjet.com")
                        .put("Name", "Mailjet Pilot"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger1@mailjet.com")
                            .put("Name", "passenger 1")))
                    .put(Emailv31.Message.SUBJECT, "Your email flight plan!")
                    .put(Emailv31.Message.TEXTPART, "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!")
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
                    .put(Emailv31.Message.MONITORINGCATEGORY, "Category1")));
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
      /// This call sends a message to one recipient with Real-time Monitoring.
      /// </summary>
      static void Main(string[] args)
      {
         RunAsync().Wait();
      }
      static async Task RunAsync()
      {
         MailjetClient client = new MailjetClient(Environment.GetEnvironmentVariable("MJ_APIKEY_PUBLIC"), Environment.GetEnvironmentVariable("MJ_APIKEY_PRIVATE"))
         {
            Version = ApiVersion.V3_1,
         };
         MailjetRequest request = new MailjetRequest
         {
            Resource = Send.Resource,
         }
            .Property(Send.Messages, new JArray {
                new JObject {
                 {"From", new JObject {
                  {"Email", "pilot@mailjet.com"},
                  {"Name", "Mailjet Pilot"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "passenger1@mailjet.com"},
                   {"Name", "passenger 1"}
                   }
                  }},
                 {"Subject", "Your email flight plan!"},
                 {"TextPart", "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!"},
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"},
                 {"MonitoringCategory", "Category1"}
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
            Console.WriteLine(response.GetData());
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


Real-time Monitoring enables you to have a constant supervision on your transactional messages by creating custom alerts on categories of Messages.

You can create as many alerts as you want under one of our four types of monitoring alerts, allowing you to fully control your sending:

 - Unusual volume: triggers a notification when emails of the chosen category are not sent/delivered at all, or the volume is unusually low over a chosen period of time.
 - Error on critical message: alerts you each time a message is blocked by our systems or bounces from the recipient’s inbox.
 - Delivery delay: notifies you when emails are delivered slower than expected.
 - Unusual statistics: sends you an alert when delivery, bounce or open rate are lower or higher than your predefined threshold.

 Mailjet’s Real-time monitoring will inform you by email, Slack or even SMS based on the alert configuration.

After creating your categories and alerts in our [user interface](https://app.mailjet.com/monitoring/overview), you can leverage the <code>MonitoringCategory</code> property to specify which category the message you are sending is related to.

Learn more about [Mailjet Real-time Monitoring](https://www.mailjet.com/feature/real-time-monitoring).

## Send API JSON properties

### Payload property

The Send API payload will contain only one property <code>Messages</code>, which is a collection of messages, represented as a JSON array.

### Message JSON properties

Each message will be described with the following properties.

Property Name | Description
  ----------|------------
From | Must be a valid, activated and registered sender for this account<br /> JSON object with 2 properties <code>Email</code> and <code>Name</code><br />Sample : <code>{"Email": "pilot@mailjet.com", "Name":"your pilot speaking"}</code>
Sender | It applies only on a given API Key. Contact the <a href="https://app.mailjet.com/support/ticket" target="_blank">support team</a> if you want us to enable this setting for the whole account.<br />Must be a valid active sender for this account.<br />Perform a simple GET on resource <code>/sender</code> to view a list of allowed senders for your account, or within the Mailjet Account Settings under <a href="https://app.mailjet.com/account/sender" target="_blank">Sender Addresses</a><br /> JSON object with 2 properties <code>Email</code> and <code>Name</code><br />Sample : <code>{"Email": "pilot@mailjet.com", "Name":"your pilot speaking"}</code>
To | Collection of recipients, each presented as a JSON object <br />Sample: <code>[{"Email":"passenger@mailjet.com","Name":"passenger"}] </code><br /> If a recipient is specified twice (in the to, cc, or bcc), it is counted only once.<br />Can be a contact list as well (magic list) - address@lists.mailjet.com. See the <code>Address</code> [contactslist](/reference/email/contacts/contact-list/) property.<br />**MAX RECIPIENTS: 50**
Cc, Bcc | Collection of carbon copy and blind carbon copy recipients, each presented as a JSON object <br />Sample: <code>[{"Email":"passenger@mailjet.com","Name":"passenger"}]</code> <br />If one recipient is specified twice, we count it as one (including to, cc, bcc)<br />**MAX RECIPIENTS: 50**<br />
ReplyTo | Reply to address for the message<br /> Email address, where the replies go to. JSON object with 2 properties <code>Email</code> and <code>Name</code><br />Sample : <code>{"Email": "pilot@mailjet.com", "Name":"your pilot listening"}</code>
Subject | Maximum length is 255 chars <br />**MAX SUBJECTS: 1**
TextPart | Provides the Text part of the message<br />Mandatory if the HTML or TemplateID parameter is not specified<br />**MANDATORY IF NO HTML - MAX PARTS: 1**
HTMLPart | Provides the HTML part of the message<br />Mandatory if the Text or TemplateID parameter is not specified<br />**MANDATORY IF NO TEXT - MAX PARTS: 1**
TemplateID | The Template ID to be used as email content. Overrides the HTML/Text parts if any.<br />Equivalent to using the <code>X-MJ-TemplateID</code> header through SMTP.<br />**MANDATORY IF NO HTML/TEXT - MAX TEMPLATEID: 1**
TemplateLanguage | Activates the template language processing. By default the template language processing is deactivated. Use <code>True</code> to activate.<br />Equivalent to using the <code>X-MJ-TemplateLanguage header</code> through SMTP.<br />[More information](#use-the-template-in-send-api)
TemplateErrorReporting | Recipient address where template language error reports will be delivered. Can only be set when <code>TemplateLanguage</code> is <code>true</code>. Format: <code>{Email, Name}</code> <br />Equivalent to using the X-MJ-TemplateErrorReporting header through SMTP.<br />[More information](#templates-error-management)
TemplateErrorDeliver | Boolean flag, determining whether the message should be delivered in case of an error while executing the template language. Can only be set when <code>TemplateLanguage</code> is <code>true</code> <br />Equivalent to using the X-MJ-TemplateErrorDeliver header through SMTP.<br />[More information](#templates-error-management)
Attachments | Attach files automatically to this Email<br />Size of all attachments, including inline may not exceed 15 MB total<br />Sample: [{"ContentType": "MIME TYPE", "Filename": "FILENAME.EXT", "Base64Content":"BASE64 ENCODED CONTENT"}]
InlinedAttachments | Attach a file for inline use via <code>cid:ID_FOR_HTML</code><br />Size of all attachments, including inline must not exceed 15 MB total<br />Sample: [{"ContentType": "MIME TYPE", "Filename": "FILENAME.EXT", "Base64Content":"BASE64 ENCODED CONTENT, "ContentID":"ID_FOR_HTML"}]
Priority | Manage message processing priority inside your account's (API key) scheduling queue. <br />Equivalent of using <code>X-Mailjet-Prio</code> header through SMTP<br /><a href="https://app.mailjet.com/docs/email-priority-management" target="_blank">More information</a>
CustomCampaign | Groups multiple messages in one campaign<br />Equivalent of using <code>X-Mailjet-Campaign</code> header through SMTP.<br /><a href="https://app.mailjet.com/docs/emails_headers" target="_blank">More information</a>
DeduplicateCampaign | Block/unblock messages to be sent multiple times inside one campaign to the same contact.<br>- <code>false</code>: unblocked (default behaviour)<br />- <code>true</code>: blocked<br />Equivalent of using <code>X-Mailjet-DeduplicateCampaign</code> header through SMTP.<br />Can only be used if <code>CustomCampaign</code> is specified.<br /><a href="#custom-headers" target="_blank">More information</a>
TrackOpens | Force or disable open tracking on this message, can override account preferences.<br />Can only be used with a HTML part.<br /> Accepts 3 values: <code> account_default </code> , <code> disabled </code>, <code> enabled </code>. Equivalent to X-Mailjet-TrackOpen SMTP custom header (despite the different format)
TrackClicks | Force or disable click tracking on this message, can override account preferences.<br />Can only be specified if the HTML part is provided.<br /> Accepts 3 values: <code> account_default </code> , <code> disabled </code>, <code> enabled </code>. Equivalent to X-Mailjet-TrackOpen SMTP custom header (despite the different format)
CustomID | Attach a custom ID to the message<br />Equivalent to using the X-MJ-CustomID header through SMTP.<br />[More information](#sending-an-email-with-a-custom-id)
EventPayload | Attach a payload to the message<br />Equivalent to using the X-MJ-EventPayload header through SMTP.<br />[More information](#sending-an-email-with-a-payload)
MonitoringCategory | Category for Real-time Monitoring to which the message will be attached
URLTags | URL parameters to append to all your URLs. A maximum length of 256 characters is allowed per message.
Headers | Add lines to the email's headers<br />List of headers as <code>"property":"value"</code> pairs<br />Notice: overriding values of header properties (ie: subject)<br />Sample: <code>{"X-My-Header":"my own value","X-My-Header-2":"my own value 2"}</code>
Variables | Variables used for personalization and/or template language in the message<br />Equivalent of using <code>X-MJ-Vars</code> header through SMTP.


## Send API errors

> Global Error (example: broken JSON format)

```json
{
  "ErrorIdentifier": "06df1144-c6f3-4ca7-8885-7ec5d4344113",
  "ErrorCode": "mj-0002",
  "ErrorMessage": "Malformed JSON, please review the syntax and properties types.",
  "StatusCode": 400
}
```

> Error in validation of the JSON Payload

```json
{
  "Messages":[
    {
      "Status": "error",
      "Errors": [
        {
          "ErrorIdentifier": "f987008f-251a-4dff-8ffc-40f1583ad7bc",
          "ErrorCode": "mj-0004",
          "StatusCode": 400,
          "ErrorMessage": "Type mismatch. Expected type \"array of emails\".",
          "ErrorRelatedTo": ["HTMLPart", "TemplateID"]
        },
        {
          "ErrorIdentifier": "8e28ac9c-1fd7-41ad-825f-1d60bc459189",
          "ErrorCode": "mj-0005",
          "StatusCode": 400,
          "ErrorMessage": "The To is mandatory but missing from the input",
          "ErrorRelatedTo": ["To"]
        }
      ]
    }
  ]
}
```

When an error occurs on a message validation, a <code>error</code> <code>Status</code> will be returned in the response. The description of the error(s) will be contain in the <code>Errors</code> property.
Each error will contain the following properties:

 - <code>ErrorIdentifier</code>: internal Mailjet Error identifier
 - <code>ErrorCode</code>: standardized classification of the error (see table bellow)
 - <code>StatusCode</code>: Status code of the error , follow the http status code  
 - <code>ErrorMessage</code>: description of the error
 - <code>ErrorRelatedTo</code>: list of message properties related to this error

In bulk sending (multiple instances of messages in <code>Messages</code>), we will process each message separately. As a result, the response can contain both success and error notifications.
For a single message, Send API can return multiple errors, each related to different properties of the payload.

Status Code | Error Code | Description | Related To
  ----------|------------|------------|------------
400 | send-0003 | when none of the <code>HTML</code>, <code>Text</code>, <code>TemplateID</code> properties are provided. | HTMLPart, TextPart, TemplateID
400 | send-0004 | when providing <code>HTML</code> property, as well as a template also containing an HTML part - i.e. Duplicated content | HTMLPart, TemplateID
403 | send-0006 | when the API key doesn’t have permission to use a Sender header. Please contact our support team to be granted permission. | SenderID
403 | send-0007 | when <code>SenderID</code> is provided but not validated. | SenderID
403 | send-0008 | when the sender email address provided in the From property is not authorized. The validation can be done on the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> page or through the <a href="https://dev.mailjet.com/guides/#verify-a-sender" target="_blank">API</a>. | From
400 | send-0010 | when the API key can’t send the provided template. Please verify the owner of the template.  | TemplateID
400 | send-0011 | when one of the forbidden headers (headers that have a property alternative) is set in the Headers collection. Please use the dedicated message property to set this header. | Headers["headerName"]
400 | send-0012 | when <code>DeduplicateCampaign</code> is set to true while no <code>CustomCampaign</code> is defined. | CustomCampaign, DeduplicateCampaign
400 | send-0013 | when <code>MonitoringCategory</code> does not exist for the API key. | MonitoringCategory
400 | send-0015 | when the total number of recipients is over the limit. | To,CC,Bcc
400 | send-0016 | when <code>TemplateLanguage</code> value is missing but <code>TemplateErrorReporting</code> or <code>TemplateErrorDeliver</code> are present. | TemplateLanguage
401 | mj-0001 | when API key is suspended. |
400 | mj-0002 | when the API call contains payload with an invalid JSON syntax. |
400 | mj-0003 | when mandatory property is missing or with null value. See <code>ErrorRelatedTo</code> for a list |
400 | mj-0005 | when a property value is not an allowed values. | Priority<br>TrackClicks<br>TrackOpens
400 | mj-0006 | when a property contains more than the maximum allowed number of characters. | Subject<br>URLTags
400 | mj-0007 | when an empty array is provided that cannot be empty. | Messages<br>To
400 | mj-0008 | when an array property contains more than the maximum allowed number of elements. | Messages<br>Attachments<br>InlineAttachments<br>Headers<br>Variables<br>
400 | mj-0011 | when the payload size is over the limit. |
400 | mj-0012 | when an empty string value is provided. | Email<br>FileName<br>Base64Content<br>ContentType
400 | mj-0013 | when the email address format is invalid. | Email
401 | mj-0015 | when the user did not provide valid authorization credentials. |

When the HTTP status for the API call is 500, you will see <code>ErrorIdentifier</code> field. It will contain a reference to the error in our internal log and it is crucial for us to determine the root cause of the failure. Should you encounter such errors, please contact our <a href="https://www.mailjet.com/support/ticket" target="_blank">support</a> for additional investigation, providing this error identifier.
