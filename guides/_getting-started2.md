# Getting Started

In this section we will guide you through the core features of the Mailjet Email API : sending an email and retrieving information about your sent messages, including engagement statistics (opens, clicks, etc.).

Let’s start!

## Prerequisites

To use the Mailjet Email API, you need to:

1. [Create a Mailjet account](https://app.mailjet.com/signup), then [retrieve your API and Secret keys](https://app.mailjet.com/account/api_keys). They will be used for authentication purposes.
2. Make sure you have [cURL](https://curl.haxx.se/) installed on your machine, or use one of our official libraries in [PHP](https://github.com/mailjet/mailjet-apiv3-php), [Python](https://github.com/mailjet/mailjet-apiv3-python), [Node.js](https://github.com/mailjet/mailjet-apiv3-nodejs), [Java](https://github.com/mailjet/mailjet-apiv3-java), [C#](https://github.com/mailjet/mailjet-apiv3-dotnet), [Go](https://github.com/mailjet/mailjet-apiv3-go), and [Ruby](https://github.com/mailjet/mailjet-gem).

    Alternatively, you can use [Postman](https://www.getpostman.com/) to test the different API endpoints. Click on the button below to import a pre-made collection of examples.

<div style="margin-left:45px;" class="postman-run-button"
data-postman-action="collection/import"
data-postman-var-1="f10958358116f86b841d"></div>
<script type="text/javascript">
  (function (p,o,s,t,m,a,n) {
    !p[s] && (p[s] = function () { (p[t] || (p[t] = [])).push(arguments); });
    !o.getElementById(s+t) && o.getElementsByTagName("head")[0].appendChild((
      (n = o.createElement("script")),
      (n.id = s+t), (n.async = 1), (n.src = m), n
    ));
  }(window, document, "_pm", "PostmanRunObject", "https://run.pstmn.io/button.js"));
</script>

We also suggest that you create environment variables for your API and Secret keys for easy input.

```bash
export $MJ_APIKEY_PUBLIC='Enter your API Key here'
export $MJ_APIKEY_PRIVATE='Enter your API Secret here'
```

- On Mac OS / Linux:

<div></div>

```bash
setx -m $MJ_APIKEY_PUBLIC "Enter your API Key here"
setx -m $MJ_APIKEY_PRIVATE "Enter your API Secret here"
```

- On Windows:

<div></div>

## Send your first email

First, define the sender and recipient email as environment variables. Keep in mind that the sender email should be [validated in your Mailjet account](https://app.mailjet.com/account/sender) (your signup email address will be automatically validated).

```bash
export $SENDER_EMAIL='Enter your sender email address here'
export $RECIPIENT_EMAIL='Enter your recipient email address here'
```

 - On Mac OS / Linux:

 <div></div>

 ```bash
 setx -m $SENDER_EMAIL "Enter your sender email address here"
 setx -m $RECIPIENT_EMAIL "Enter your recipient email address here"
 ```

 - On Windows:

 <div></div>

```shell
# Run:
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d '{
		"Messages":[
				{
						"From": {
								"Email": "$SENDER_EMAIL",
								"Name": "Me"
						},
						"To": [
								{
										"Email": "$RECIPIENT_EMAIL",
										"Name": "You"
								}
						],
						"Subject": "My first Mailjet Email!",
						"TextPart": "Greetings from Mailjet!",
						"HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
				}
		]
	}'
```
```php
<?php
/*
Run:
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'),true,['version' => 'v3.1']);
$body = [
    'Messages' => [
        [
            'From' => [
                'Email' => "$SENDER_EMAIL",
                'Name' => "Me"
            ],
            'To' => [
                [
                    'Email' => "$RECIPIENT_EMAIL",
                    'Name' => "You"
                ]
            ],
            'Subject' => "My first Mailjet Email!",
            'TextPart' => "Greetings from Mailjet!",
            'HTMLPart' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!</h3><br />May the delivery force be with you!"
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
 * Run:
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
								"Email": "$SENDER_EMAIL",
								"Name": "Me"
						},
						"To": [
								{
										"Email": "$RECIPIENT_EMAIL",
										"Name": "You"
								}
						],
						"Subject": "My first Mailjet Email!",
						"TextPart": "Greetings from Mailjet!",
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
# Run:
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
  config.api_version = "v3.1"
end
variable = Mailjet::Send.create(messages: [{
    'From'=> {
        'Email'=> '$SENDER_EMAIL',
        'Name'=> 'Me'
    },
    'To'=> [
        {
            'Email'=> '$RECIPIENT_EMAIL',
            'Name'=> 'You'
        }
    ],
    'Subject'=> 'My first Mailjet Email!',
    'TextPart'=> 'Greetings from Mailjet!',
    'HTMLPart'=> '<h3>Dear passenger 1, welcome to <a href=\'https://www.mailjet.com/\'>Mailjet</a>!</h3><br />May the delivery force be with you!'
}]
)
p variable.attributes['Messages']
```
```python
"""
Run:
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
								"Email": "$SENDER_EMAIL",
								"Name": "Me"
						},
						"To": [
								{
										"Email": "$RECIPIENT_EMAIL",
										"Name": "You"
								}
						],
						"Subject": "My first Mailjet Email!",
						"TextPart": "Greetings from Mailjet!",
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
Run:
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
          Email: "$SENDER_EMAIL",
          Name: "Me",
        },
        To: &mailjet.RecipientsV31{
          mailjet.RecipientV31 {
            Email: "$RECIPIENT_EMAIL",
            Name: "You",
          },
        },
        Subject: "My first Mailjet Email!",
        TextPart: "Greetings from Mailjet!",
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
     * Run:
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
                        .put("Email", "$SENDER_EMAIL")
                        .put("Name", "Me"))
                    .put(Emailv31.Message.TO, new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "$RECIPIENT_EMAIL")
                            .put("Name", "You")))
                    .put(Emailv31.Message.SUBJECT, "My first Mailjet Email!")
                    .put(Emailv31.Message.TEXTPART, "Greetings from Mailjet!")
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
      /// Run:
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
                  {"Email", "$SENDER_EMAIL"},
                  {"Name", "Me"}
                  }},
                 {"To", new JArray {
                  new JObject {
                   {"Email", "$RECIPIENT_EMAIL"},
                   {"Name", "You"}
                   }
                  }},
                 {"Subject", "My first Mailjet Email!"},
                 {"TextPart", "Greetings from Mailjet!"},
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


Then use the code sample to send a message.

<div></div>

> Example API Response:

```json
{
  "Messages": [
    {
      "Status": "success",
      "To": [
        {
          "Email": "passenger@mailjet.com",
          "MessageUUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
          "MessageID": "1234567890987654321",
          "MessageHref": "https://api.mailjet.com/v3/message/1234567890987654321"
        }
      ],
    }
  ]
}
```

**Congratulations - you have successfully sent your first email!**

Save the `MessageID` - we will need it in the next section to access detailed information about the sent email.

<div></div>

## Retrieve sent messages

```shell
# Run :
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message/$MESSAGE_ID 
```
```php
<?php
/*
Run :
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
 * Run :
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("message")
	.id($MESSAGE_ID)
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
# Run :
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Message.find($MESSAGE_ID)
p variable.attributes['Data']
```
``` go
/*
Run :
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
```python
"""
Run :
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$MESSAGE_ID'
result = mailjet.message.get(id=id)
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
import com.mailjet.client.resource.Message;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Run :
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
      /// Run :
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


Now, let’s view the status of the sent message and its configuration specifics.

<div></div>

> Example API response:

```json
{
  "Count": "1",
  "Data": [
    {
      "ArrivedAt": "2018-01-01T00:00:00",
      "AttachmentCount": "1",
      "AttemptCount": "1",
      "CampaignID": "123456789",
      "ContactAlt": "",
      "ContactID": "987654",
      "FilterTime": "111",
      "ID": "1234567890987654321",
      "Status": "clicked",
      "Subject": "",
      "UUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j"
      "........................."
    }
  ],
  "Total": "1"
}
```

As you can see, the information about the message is quite detailed and includes, among other things:

- The time the message arrived
- The contact, to which it was sent
- Message size
- Number of attachments
- Current message status (e.g. sent, opened, clicked, etc.)

In case you want to view the contact email address, or the Subject of the email, add the `ShowContactAlt=true` and `ShowSubject=true` as query parameters, respectively.

<div></div>

```shell
# Run :
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message?ContactAlt=$RECIPIENT_EMAIL 
```
```php
<?php
/*
Run :
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'ContactAlt' => '$RECIPIENT_EMAIL'
];
$response = $mj->get(Resources::$Message, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * Run :
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("message")
	.request({
		"ContactAlt":"$RECIPIENT_EMAIL"
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
# Run :
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Message.all(contact_alt: "$RECIPIENT_EMAIL"
)
p variable.attributes['Data']
```
```python
"""
Run :
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'ContactAlt': '$RECIPIENT_EMAIL'
}
result = mailjet.message.get(filters=filters)
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
import com.mailjet.client.resource.Message;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Run :
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Message.resource)
                  .filter(Message.CONTACTALT, "$RECIPIENT_EMAIL");
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
Run :
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
	_, _, err := mailjetClient.List("message", &data, Filter("ContactAlt", "$RECIPIENT_EMAIL"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
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
      /// Run :
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
         .Filter(Message.Contactalt, "$RECIPIENT_EMAIL");
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


Alternatively, you can retrieve messages sent to a specific recipient email address, using a query parameter:

<div></div>

## View message history

```shell
# Run :
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messagehistory/$MESSAGE_ID 
```
```php
<?php
/*
Run :
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messagehistory, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * Run :
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("messagehistory")
	.id($MESSAGE_ID)
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
# Run :
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Messagehistory.find($MESSAGE_ID)
p variable.attributes['Data']
```
``` go
/*
Run :
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
```python
"""
Run :
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$MESSAGE_ID'
result = mailjet.messagehistory.get(id=id)
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
import com.mailjet.client.resource.Messagehistory;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Run :
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
      /// Run :
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


You can track important events linked to the sent emails, for example whether the recipient opened the message, or clicked on a link within.

Do a `GET` request on [`/messagehistory/{message_ID}`](https://dev.mailjet.com/reference/email/messages/#v3_get_messagehistory_message_ID) to retrieve the list of events linked to the email we just sent.

<div></div>

> Example API Response:

```json
{
    "Count": 4,
    "Data": [
        {
            "EventAt": 1546958313,
            "EventType": "sent",
            ".........................."
        },
        {
            "EventAt": 1546958354,
            "EventType": "opened",
            ".........................."
        },
        {
            "EventAt": 1546958355,
            "EventType": "clicked",
            ".........................."
        }
    ],
    "Total": 4
}
```

We can see the event type, as well as a timestamp indicating when it happened. Pretty useful, right?

<aside class="notice">
Since the API detects each email event, you are able to see multiple events of the same type, for example when the message is opened multiple times, or there are multiple link clicks.
</aside>

<div></div>

## Retrieve Statistics

```php
<?php
/*
Run :
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'CounterSource' => 'APIKey',
  'CounterTiming' => 'Message',
  'CounterResolution' => 'Lifetime'
];
$response = $mj->get(Resources::$Statcounters, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * Run :
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("statcounters")
	.request({
		"CounterSource":"APIKey",
		"CounterTiming":"Message",
		"CounterResolution":"Lifetime"
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
```shell
# Run :
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/statcounters?CounterSource=APIKey\&CounterTiming=Message\&CounterResolution=Lifetime 
```
```ruby
# Run :
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Statcounters.all(counter_source: "APIKey",
counter_timing: "Message",
counter_resolution: "Lifetime"
)
p variable.attributes['Data']
```
```python
"""
Run :
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'CounterSource': 'APIKey',
  'CounterTiming': 'Message',
  'CounterResolution': 'Lifetime'
}
result = mailjet.statcounters.get(filters=filters)
print result.status_code
print result.json()
```
``` go
/*
Run :
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
	var data []resources.Statcounters
	_, _, err := mailjetClient.List("statcounters", &data, Filter("CounterSource", "APIKey"), Filter("CounterTiming", "Message"), Filter("CounterResolution", "Lifetime"))
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
import com.mailjet.client.resource.Statcounters;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Run :
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Statcounters.resource)
                  .filter(Statcounters.COUNTERSOURCE, "APIKey")
                  .filter(Statcounters.COUNTERTIMING, "Message")
                  .filter(Statcounters.COUNTERRESOLUTION, "Lifetime");
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
      /// Run :
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
            Resource = Statcounters.Resource,
         }
         .Filter(Statcounters.Countersource, "APIKey")
         .Filter(Statcounters.Countertiming, "Message")
         .Filter(Statcounters.Counterresolution, "Lifetime");
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


> Example API Response

```json
{
  "Count": "1",
  "Data": [
    {
      "APIKeyID": "123456",
      "MessageClickedCount": "1",
      "MessageDeferredCount": "0",
      "MessageHardBouncedCount": "0",
      "MessageOpenedCount": "1",
      "MessageQueuedCount": "0",
      "MessageSentCount": "1",
      "MessageSoftBouncedCount": "0",
      "MessageSpamCount": "0",
      "MessageUnsubscribedCount": "0",
      "Total": "1"
      "........................."
    }
  ],
  "Total": "1"
}
```

The Mailjet API also has a variety of resources that help retrieve aggregated statistics for key performance indicators like opens, clicks, unsubscribes, etc.

Let's take a look at just one of those resources to give you a sample of the data you can read - we’ll retrieve total aggregated statistics for your API key.

<div></div>

## Next steps

You have played around with some of the basic functionalities of the Mailjet API, but there is so much more to learn! Continue reading to understand how to make the most out of the available resources.

Here's a small list of suggestions:

- [Verify Your Domain](#verify-your-domain) - Complete the SPF / DKIM authentication of your sender domains, which will ensure higher deliverability of your emails.
- [Send API v3.1](#send-api-v3-1) - Get acquainted with the full capabilities of our Send API - sending in bulk, using templates, adding headers and much more.
- [Event Tracking](#event-api-real-time-notifications) - Learn how to track email events (open, click, unsubscribe, bounce, etc.) in almost real time with the Mailjet Event API.
- [Statistics](#statistics) - See the numerous possibilities to extract different delivery and engagement metrics.

For full information on all API endpoints, please visit our [API Reference](https://dev.mailjet.com/reference/email/).
