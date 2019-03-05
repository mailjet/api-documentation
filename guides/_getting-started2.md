# Getting Started

In this section we will introduce you to the basics of the Mailjet Email API -  sending your first email, retrieving your sent messages and deliverability / contact engagement statistics.

Let’s start!

## Prerequisites

To use the Mailjet Email API, you need to:

- [Create a Mailjet account](https://app.mailjet.com/signup), then [retrieve your API and Secret keys](https://app.mailjet.com/account/api_keys). They will be used for authentication purposes.
- Make sure you have [curl](https://curl.haxx.se/) installed on your machine. We also have [PHP](https://github.com/mailjet/mailjet-apiv3-php), [Python](https://github.com/mailjet/mailjet-apiv3-python), [Node.js](https://github.com/mailjet/mailjet-apiv3-nodejs), [Java](https://github.com/mailjet/mailjet-apiv3-java), [C#](https://github.com/mailjet/mailjet-apiv3-dotnet), [Go](https://github.com/mailjet/mailjet-apiv3-go), and [Ruby](https://github.com/mailjet/mailjet-gem) libraries you can install for easy use of the API. Alternatively, you can use [Postman](https://www.getpostman.com/) to make your calls.

## Send your first email

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
            'HTMLPart' => "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
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
						"HTMLPart": "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
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
						"HTMLPart": "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
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
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!'
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
						"HTMLPart": "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"
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
        HTMLPart: "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!",
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
                    .put(Emailv31.Message.HTMLPART, "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!")));
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
                 {"HTMLPart", "<h3>Dear passenger 1, welcome to Mailjet!</h3><br />May the delivery force be with you!"}
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


Let’s go ahead and send our first email.

Use the below code sample to send an email, after entering the required details:

 - `$MJ_APIKEY_PUBLIC` : Your API Key
 - `$MJ_APIKEY_PRIVATE` : Your Secret Key
 - `$SENDER_EMAIL` : The sender email address you want to use (must be validated in your [Mailjet account](https://app.mailjet.com/account/sender))
 - `$RECIPIENT_EMAIL` : The recipient email address for your test

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

The response will indicate a successful sending, showing the ID of the sent message. Congrats!

Save the message ID - you can use it later to access detailed information about it.

<div></div>

## Retrieve sent messages

```shell
# View : Details of a specific Message (e-mail) processed by Mailjet
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message/$ID_MESSAGE 
```
```php
<?php
/*
View : Details of a specific Message (e-mail) processed by Mailjet
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Message, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * View : Details of a specific Message (e-mail) processed by Mailjet
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("message")
	.id($ID_MESSAGE)
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
# View : Details of a specific Message (e-mail) processed by Mailjet
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Message.find($ID_MESSAGE)
p variable.attributes['Data']
```
```python
"""
View : Details of a specific Message (e-mail) processed by Mailjet
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_MESSAGE'
result = mailjet.message.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : Details of a specific Message (e-mail) processed by Mailjet
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
	mr := &Request{
	  Resource: "message",
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
import com.mailjet.client.resource.Message;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Details of a specific Message (e-mail) processed by Mailjet
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Message.resource, ID);
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
      /// View : Details of a specific Message (e-mail) processed by Mailjet
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
            ResourceId = ResourceId.Numeric(ID)
         }
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"ArrivedAt": "2015-07-06T07:10:24Z",
			"AttachmentCount": "0",
			"AttemptCount": "0",
			"CampaignID": "51",
			"ContactID": "45",
			"Delay": "0",
			"DestinationID": "14",
			"FilterTime": "61",
			"FromID": "1",
			"ID": "16888509234525280",
			"IsClickTracked": "false",
			"IsHTMLPartIncluded": "false",
			"IsOpenTracked": "false",
			"IsTextPartIncluded": "false",
			"IsUnsubTracked": "false",
			"MessageSize": "20248",
			"SpamassassinScore": "0",
			"SpamassRules": "",
			"StateID": "0",
			"StatePermanent": "false",
			"Status": "sent"
		}
	],
	"Total": 1
}
```


Now that we’ve sent our first email, we are able to retrieve the message via the API. Let’s retrieve the message to view its configuration specifics and current status.

We can use the message ID we got in the response after the send request to get that specific message.

<div></div>

```php
<?php
/*
View : Event history of a message.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messagehistory, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : Event history of a message.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messagehistory/$ID_MESSAGE 
```
```javascript
/**
 *
 * View : Event history of a message.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("messagehistory")
	.id($ID_MESSAGE)
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
# View : Event history of a message.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Messagehistory.find($ID_MESSAGE)
p variable.attributes['Data']
```
```python
"""
View : Event history of a message.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_MESSAGE'
result = mailjet.messagehistory.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : Event history of a message.
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
	var data []resources.Messagehistory
	mr := &Request{
	  Resource: "messagehistory",
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
import com.mailjet.client.resource.Messagehistory;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Event history of a message.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Messagehistory.resource, ID);
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
      /// View : Event history of a message.
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
            Resource = Messagehistory.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
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


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"Comment": "",
			"EventAt": "",
			"EventType": "sent",
			"State": "",
			"Useragent": ""
		}
	],
	"Total": 1
}
```


If we have multiple sent messages, we can use a query parameter to get a list of messages based on specific criteria - for example, ones sent to a specific recipient.

## View message history

[code sample]

Mailjet has the ability to track important events linked to your sent emails - e.g. whether the message was opened by the recipient, or a link within it was clicked.

Do a `GET` request on [`/messagehistory/{message_ID}`](https://dev.mailjet.com/reference/email/messages/#v3_get_messagehistory_message_ID) to retrieve the list of events linked to the email we just sent.

<div></div>

## Retrieve Statistics

[statcounters code sample]

The Mailjet API has a variety of resources that help you retrieve your deliverability and engagement statistics.

Let’s take a look at just one of those - we’ll retrieve aggregated statistics for your API key.

<div></div>

## Next steps

Now that you are familiar with some of the basic functionalities of the Mailjet API, it’s time to review the resources in more details. See the below sections for comprehensive information:

[links to sendapi, contacts, stats, eventapi etc]
