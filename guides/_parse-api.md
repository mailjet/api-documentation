<div id="receive-emails"></div>
<div id="#parse-api-process-inbound-emails"></div>

# Parse API: inbound emails

<aside class="warning">
Parse API is now available only for users on paid Mailjet plans (Crystal and above). Upgrade now to keep using Parse API.
<br>Check <a href="https://app.mailjet.com/subscription" target="_blank">plans</a> and <a href="https://www.mailjet.com/pricing/" target="_blank">pricing</a>.
</aside>

The Parse API allows you to have inbound emails parsed and their content delivered to a webhook of your choice.

It will make the processing of inbound messages easier as Mailjet will do all the job of sifting through and organizing all the information in headers, content and attachments. What's left to do is just to save the information in your CRM or database.

## Basic Setup

```shell
# Create : ParseRoute description
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/parseroute \
	-H 'Content-Type: application/json' \
	-d '{
		"Url":"https://www.mydomain.com/mj_parse.php"
	}'
```
```javascript
/**
 *
 * Create : ParseRoute description
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("parseroute")
	.request({
		"Url":"https://www.mydomain.com/mj_parse.php"
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```php
<?php
/*
Create : ParseRoute description
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Url' => "https://www.mydomain.com/mj_parse.php"
];
$response = $mj->post(Resources::$Parseroute, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```ruby
# Create : ParseRoute description
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Parseroute.create(url: "https://www.mydomain.com/mj_parse.php"
)
p variable.attributes['Data']
```
```python
"""
Create : ParseRoute description
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Url': 'https://www.mydomain.com/mj_parse.php'
}
result = mailjet.parseroute.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : ParseRoute description
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
	var data []resources.Parseroute
	mr := &Request{
	  Resource: "parseroute",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Parseroute {
      Url: "https://www.mydomain.com/mj_parse.php",
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
import com.mailjet.client.resource.Parseroute;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : ParseRoute description
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Parseroute.resource)
			.property(Parseroute.URL, "https://www.mydomain.com/mj_parse.php");
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
      /// Create : ParseRoute description
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
            Resource = Parseroute.Resource,
         }
            .Property(Parseroute.Url, "https://www.mydomain.com/mj_parse.php");
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


> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"APIKeyID": "11111111",
			"Email": "16Hy-aOYhApIzTgN@parse-in1.mailjet.com",
			"ID": "1",
			"Url": "https://www.mydomain.com/mj_parse.php"
		}
	],
	"Total": 1
}
```


In order to begin receiving emails to your webhook, create a new instance of the Parse API via a <code>POST</code> request on the <code>[/parseroute](/reference/email/parse/#v3_post_parseroute)</code> resource. This call has only one mandatory property - the <code>Url</code> of the webhook (Note: URLs provided cannot be the root). Mailjet will provide an <code>Email</code> address (on the subdomain @parse-in1.mailjet.com) in the response, you can begin to use immediately. It can be used as a Reply-to Email address for example.

We strongly recommend using a secure (HTTPS) URL in combination with a basic authentification to make sure data cannot be intercepted, and that only our servers can send you data.

E.g.: <code>https://username:password@www.example.com/mailjet_parser.php</code>

You can also specify a port in your webhook URL.

E.g.: <code>https://www.example.com:123/mailjet_triggers.php</code>

<aside class="notice">
Mailjet provides only one Email address (on the subdomain @parse-in1.mailjet.com) per API key.
</aside>

Parse API also allows you to use your own email address and domain using the <code>Email</code> property in the <code>/parseroute</code> resource. Visit [Use your own domain](#use-your-own-domain) section for more information on how to setup your DNS and your parseroute.

## What is delivered to your webhook

```json
{
	"Sender":"pilot@mailjet.com",
	"Recipient":"passenger@mailjet.com",
	"Date":"20150410T160638",
	"From":"Pilot <pilot@mailjet.com>",
	"Subject":"Hey! It's Friday!",
	"Headers":{
		"Return-Path": [
			"<pilot@mailjet.com>"
		],
		"Received": [
			"by 10.107.134.160 with HTTP; Fri, 10 Apr 2015 09:06:38 -0700 (PDT)"
		],
		"DKIM-Signature": [
			"v=1; a=rsa-sha256; c=relaxed/relaxed;        d=mailjet.com; s=google;        h=mime-version:date:message-id:subject:from:to:content-type;        bh=tsc4ruu5r5loLtAFUwhFp8BIbKzV0AYljT0+Bb/QwWI=;        b=............"
		],
		"MIME-Version": [
			"1.0"
		],
		"Content-Transfer-Encoding": [
			"quoted-printable"
		],
		"Content-Type": [
			"multipart/alternative; boundary=001a1141f3c406f1b2051360f37d"
		],
		"X-CSA-Complaints": [
			"whitelist-complaints@eco.de"
		],
		"List-Unsubscribe": [
			"<mailto:unsub-e7221da9.org1.x61425y8x4pt@bnc3.mailjet.com>"
		],
		"X-Google-DKIM-Signature": [
			"v=1; a=rsa-sha256; c=relaxed/relaxed;        d=1e100.net; s=20130820;        h=x-gm-message-state:mime-version:date:message-id:subject:from:to         :content-type;        bh=tsc4ruu5r5loLtAFUwhFp8BIbKzV0AYljT0+Bb/QwWI=;        b=..........."
		],
		"X-Gm-Message-State": [
			"ALoCoQlJBEYSiauMbHc8RXQpv3sUJvPmYAd7exYJKZIZFRZtFkSHqDEP59rQK6oIp9mCwPKCirCL"
		],
		"X-Received": [
			"by 10.107.41.72 with SMTP id p69mr3774075iop.58.1428681998638; Fri, 10 Apr 2015 09:06:38 -0700 (PDT)"
		],
		"Date":"Fri, 10 Apr 2015 18:06:38 +0200",
		"Message-ID":"<CAE5Zh0ZpHZ6G5DC+He5426a4RkVab7uWaTDwiMcHzOR=YB3urA@mail.gmail.com>",
		"Subject":"Hey! It's Friday!",
		"From":"Pilot <pilot@mailjet.com>",
		"To":"passenger@mailjet.com"
	},
	"Parts":[
		{
			"Headers":{
				"Content-Type":"text/plain; charset=UTF-8"
			},
			"ContentRef":"Text-part"
		},
		{
			"Headers":{
				"Content-Type":"text/html; charset=UTF-8",
				"Content-Transfer-Encoding":"quoted-printable"
			},
			"ContentRef":"Html-part"
		}
	],
	"Text-part":"Hi,\n\nImportant notice: it's Friday. Friday *afternoon*, even!\n\n\nHave a *great* weekend!\nThe Anonymous Friday Teller\n",
	"Html-part":"<div dir=\"ltr\">Hi,<div><br></div><div>Important notice: it&#39;s Friday. Friday <i>afternoon</i>, even!</div><div><br><br></div><div>Have a <i style=\"font-weight:bold\">great</i> weekend!</div><div>The Anonymous Friday Teller</div></div>\n",
	"SpamAssassinScore":"0.602",
	"CustomID":"helloworld",
	"Payload":"{'message': 'helloworld'}"
}
```

When an email is sent to the email address associated with your instance of the Parse API, the contents of this email will be delivered to your webhook in a JSON format.  Note that the spam score of the email is delivered in the payload via <code>SpamAssassin</code>.

This payload contains a lot of useful information about the message processed.

This payload is built following a structure where you can parse it and use key information by pointing to them directly (<code>From</code>, <code>Subject</code>, etc.).

In case you need to loop over every <code>Headers</code> or <code>Parts</code>, you can also use the related collections and loop over it.

In the <code>Parts</code> collection, the <code>ContentRef</code> property is here to link a specific part (<code>Html</code> or <code>Text</code> for instance) to its associated headers.

Also, note that the <code>CustomID</code> and the <code>Payload</code> properties are returned back if they were provided in the original message sent through the [Send API](#send-transactional-email).

Finally, be advised that most <code>Headers</code> will be provided as arrays containing the multiple header lines of the parsed email. Some headers in the payload will be represented by single strings:

 - Date
 - From
 - Sender
 - Reply-To
 - To
 - Cc
 - Bcc
 - Message-ID
 - In-Reply-To
 - References
 - Subject
 - Date



## Manage attachments

```json
{
   "Parts": [
      {
         "Headers": {
            "Content-Type": "text/plain; charset=utf-8; name=helloworld.txt",
            "Content-Transfer-Encoding": "base64",
            "Content-Disposition": "attachment; filename=helloworld.txt"
         },
         "ContentRef": "Attachment1"
      }
   ],
   "Attachment1": "SGVsbG8gV29ybGQh"
}
```

In the payload, there is a <code>Parts</code> array. This collection contains every part of the parsed message. This collection relates directly to how an email is represented in <code>Content multipart</code>.

If the parsed message contains attachments, they will be also included in the payload.

To retrieve them, you can either loop on the <code>Parts</code> collection and look for any part where the <code>ContentRef</code> property starts with Attachment or you can look directly for the <code>AttachmentN</code> property (where N is the ID of the attachment in the message, following their order).

The content of the attachments is always encoded in <a href="http://en.wikipedia.org/wiki/Base64" target="_blank">Base64</a>. <code>Content-Transfer-Encoding</code> indicates the original encoding of the attachment.


For instance, in a message containing one attachment of type <code>text/plain</code> containing "Hello World!", we will have this payload.

## CustomID and Payload

When using the Send API <code>[Mj-CustomID](#sending-an-email-with-a-custom-id)</code> or <code>[Mj-EventPayLoad](#sending-an-email-with-a-payload)</code>, the Parse API will return the values in the payload under the properties <code>CustomID</code> and <code>Payload</code>.

CustomId and Payload can be used for example to trace the conversation around your transactional emails.

<div id="use-your-own-custom-domain-name"></div>

## Use your own domain

```php
<?php
/*
Create : ParseRoute description
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Url' => "https://www.mydomain.com/mj_parse.php",
    'Email' => "mjparse@mydomain.com"
];
$response = $mj->post(Resources::$Parseroute, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : ParseRoute description
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/parseroute \
	-H 'Content-Type: application/json' \
	-d '{
		"Url":"https://www.mydomain.com/mj_parse.php",
		"Email":"mjparse@mydomain.com"
	}'
```
```javascript
/**
 *
 * Create : ParseRoute description
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("parseroute")
	.request({
		"Url":"https://www.mydomain.com/mj_parse.php",
		"Email":"mjparse@mydomain.com"
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
# Create : ParseRoute description
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Parseroute.create(url: "https://www.mydomain.com/mj_parse.php",
email: "mjparse@mydomain.com"
)
p variable.attributes['Data']
```
```python
"""
Create : ParseRoute description
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Url': 'https://www.mydomain.com/mj_parse.php',
  'Email': 'mjparse@mydomain.com'
}
result = mailjet.parseroute.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : ParseRoute description
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
	var data []resources.Parseroute
	mr := &Request{
	  Resource: "parseroute",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Parseroute {
      Url: "https://www.mydomain.com/mj_parse.php",
      Email: "mjparse@mydomain.com",
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
import com.mailjet.client.resource.Parseroute;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : ParseRoute description
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Parseroute.resource)
			.property(Parseroute.URL, "https://www.mydomain.com/mj_parse.php")
			.property(Parseroute.EMAIL, "mjparse@mydomain.com");
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
      /// Create : ParseRoute description
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
            Resource = Parseroute.Resource,
         }
            .Property(Parseroute.Url, "https://www.mydomain.com/mj_parse.php")
            .Property(Parseroute.Email, "mjparse@mydomain.com");
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


> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"APIKeyID": "11111111",
			"Email": "mjparse@mydomain.com",
			"ID": "2",
			"Url": "https://www.mydomain.com/mj_parse.php"
		}
	],
	"Total": 1
}
```


To receive emails on your own domain name, set this domain in your [parseroute](/reference/email/parse/) instance. Then, add an MX entry on the domain or subdomain DNS to <code>parse.mailjet.com.</code> (final dot is important) and specify your email address based on your domain in the <code>Email</code> attribute.

<aside class="notice">
Your domain name needs to be a verified domain. Use the <a href="https://app.mailjet.com/account/sender/domain" target="_blank">Account setting</a> page or follow the <a href="#validate-sender-and-domain">Domains and DNS</a> guide to verify your domain.
</aside>

A less intrusive alternative is to setup a mail forwarding between your current mailbox to the Parse API send-to email automatically provided by Mailjet


To use a custom domain name and email address with the Parse API, update your instance via a PUT request with the email you wish to use.
