# Send API v3

<h2 id="sending-a-basic-email-v3">Send a basic email</h2>

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
	'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
 - <code>Text-part</code> and/or <code>Html-part</code>: content of the message sent in text or HTML format. At least one of these content type needs to be specified. When Html-part is the only content provided, Mailjet will not generate a Text-part from the HTML version.   

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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
    'To' => "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",
    'CC' => "Name3 <passenger@mailjet.com>"
];
$response = $mj->post(Resources::$Send, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
variable = Mailjet::Send.create(from_email: "pilot@mailjet.com",from_name: "Mailjet Pilot",subject: "Your email flight plan!",text_part: "Dear passenger, welcome to Mailjet! May the delivery force be with you!",html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",to: "Name <passenger@mailjet.com>, Name2 <passenger2@mailjet.com>",cc: "Name3 <passenger@mailjet.com>")
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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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


<code>MessageID</code> is the internal Mailjet ID of your message. You will be able to use this id to get more information about your message. You can find more information in our [Message Statistics Guide](/guides/#messages)


<aside class="notice">
NOTICE: If a recipient does not exist in any of your contact list it will be created from scratch, keep that in mind if you are planning on sending a welcome email and then you're trying to add the email to a list as the contact effectively exists already.
</aside>

<div id="sending-to-multiple-recipients"></div>

## Send to multiple recipients

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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com', 'Name' => 'passenger 1'}, {'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}])
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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

<h2 id="sending-with-attached-files-v3">Send with attached files</h2>

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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!May the delivery force be with you!',
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
    attachments: [{'Content-Type' => 'text/plain', 'Filename' => 'test.txt', 'content' => 'VGhpcyBpcyB5b3VyIGF0dGFjaGVkIGZpbGUhISEK'}])
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
  'Html-part': '<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
  'Recipients': [ { "Email": "passenger@mailjet.com" }],
  'Inline_attachments': [
				{
						"Content-type": "image/png",
						"Filename": "logo.png",
						"content": "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")))
						.property(Email.INLINE_ATTACHMENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Content-type", "image/png")
                    .put("Filename", "logo.png")
                    .put("content", "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg==")));
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
    'Html-part' => "<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Inline_attachments' => [
        [
            'Content-type' => "image/png",
            'Filename' => "logo.png",
            'content' => "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="
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
		html_part: "<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
    'inline-attachments' => [{'Content-type' => 'image/png', 'Filename' => 'logo.png', 'content' => 'iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=='}])
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Inline_attachments":[{"Content-type":"image/png","Filename":"logo.png","content":"iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="}]
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
		"Html-part":"<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Inline_attachments":[{"Content-type":"image/png","Filename":"logo.png","content":"iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="}]
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
      HTMLPart: "<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      InlineAttachments: []MailjetAttachment {
        MailjetAttachment {
          ContentType: "image/png",
          Filename: "logo.png",
          Content: "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg==",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <img src=\"cid:logo.png\"> <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
            .Property(Send.Recipients, new JArray {
                new JObject {
                 {"Email", "passenger@mailjet.com"}
                 }
                })
            .Property(Send.InlineAttachments, new JArray {
                new JObject {
                 {"Content-type", "image/png"},
                 {"Filename", "logo.png"},
                 {"content", "iVBORw0KGgoAAAANSUhEUgAAABQAAAALCAYAAAB/Ca1DAAAACXBIWXMAAA7EAAAOxAGVKw4bAAAAB3RJTUUH4wIIChcxurq5eQAAAAd0RVh0QXV0aG9yAKmuzEgAAAAMdEVYdERlc2NyaXB0aW9uABMJISMAAAAKdEVYdENvcHlyaWdodACsD8w6AAAADnRFWHRDcmVhdGlvbiB0aW1lADX3DwkAAAAJdEVYdFNvZnR3YXJlAF1w/zoAAAALdEVYdERpc2NsYWltZXIAt8C0jwAAAAh0RVh0V2FybmluZwDAG+aHAAAAB3RFWHRTb3VyY2UA9f+D6wAAAAh0RVh0Q29tbWVudAD2zJa/AAAABnRFWHRUaXRsZQCo7tInAAABV0lEQVQokaXSPWtTYRTA8d9N7k1zm6a+RG2x+FItgpu66uDQxbFurrr5OQQHR9FZnARB3PwSFqooddAStCBoqmLtS9omx+ESUXuDon94tnP+5+1JYm057GyQjZFP+l+S6G2FzlNe3WHtHc2TNI8zOlUUGLxsD1kDyR+EEQE2P/L8Jm/uk6RUc6oZaYM0JxtnpEX9AGPTtM6w7yzVEb61EaSNn4QD3j5m4QabH6hkVFLSUeqHyCeot0ib6BdNVGscPM/hWWr7S4Tw9TUvbpFUitHTnF6XrS+sL7O6VBSausT0FZonSkb+nZUFFm+z8Z5up5Btr1Lby7E5Zq4yPrMrLR263ZV52g+LvfW3iy6PXubUNVrnhqYNF3bmiZ1i1MmLnL7OxIWh4T+IMpYeRNyrRzyZjWg/ioh+aVgZu4WfXxaixbsRve5fiwb8epTo8+kZjSPFf/sHvgNC0/mbjJbxPAAAAABJRU5ErkJggg=="}
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

<h2 id="sending-in-bulk-v3">Send in bulk</h2>

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
		"Html-part": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
	  },
	  {
		"FromEmail": "pilot@mailjet.com",
		"FromName": "Mailjet Pilot",
		"Recipients": [{ "Email": "passenger2@mailjet.com", "Name": "passenger 2" }],
		"Subject": "Your email flight plan!",
		"Text-part": "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
		"Html-part": "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
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
                    .put("Html-part", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"))
                .put(new JSONObject()
                    .put("FromEmail", "pilot@mailjet.com")
                    .put("FromName", "Mailjet Pilot")
                    .put("Recipients", new JSONArray()
                        .put(new JSONObject()
                            .put("Email", "passenger2@mailjet.com")
                            .put("Name", "passenger 2")))
                    .put("Subject", "Your email flight plan!")
                    .put("Text-part", "Dear passenger 2, welcome to Mailjet! May the delivery force be with you!")
                    .put("Html-part", "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")));
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
            'Html-part' => "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
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
            'Html-part' => "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
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
```shell
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
			"Html-part":"<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
			},
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger2@mailjet.com","Name":"passenger 2"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
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
			"Html-part":"<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
			},
			{
			"FromEmail":"pilot@mailjet.com",
			"FromName":"Mailjet Pilot",
			"Recipients":[{"Email":"passenger2@mailjet.com","Name":"passenger 2"}],
			"Subject":"Your email flight plan!",
			"Text-part":"Dear passenger 2, welcome to Mailjet! May the delivery force be with you!",
			"Html-part":"<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
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
          HTMLPart: "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
          HTMLPart: "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
                 {"Html-part", "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"}
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
                 {"Html-part", "<h3>Dear passenger 2, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"}
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

<div id="personalisation-v3"></div>
<h2 id="personalization-v3">Personalization</h2>
<div></div>
<h3 id="content-formatting-v3">Content formatting</h3>

Mailjet system allows to insert data in your text or html parts.

To do so, use <code>[[DATA_TYPE:DATA_NAME]]</code> where:

- <code>DATA_TYPE</code>: <code>var</code> for Vars specified in the API call or <code>data</code> for contact data already available on Mailjet system
- <code>DATA_NAME</code>: name of the data you want to insert

<div></div>
<h3 id="using-vars-and-custom-vars-v3">Use vars and custom vars </h3>

``` python
"""
This calls sends an email to 2 given recipient with global personalization.
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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!',
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
     * This calls sends an email to 2 given recipient with global personalization.
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day:\"monday\"]], may the delivery force be with you!")
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
This calls sends an email to 2 given recipient with global personalization.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!",
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
  'Html-part' => '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!',
  'Vars' => {'day' => 'Monday'},
  'Recipients' => [
      {'Email' => 'passenger1@mailjet.com','Name' => 'passenger 1'},
      {'Email' => 'passenger2@mailjet.com','Name' => 'passenger 2'}
  ]
)
```
```shell
# This calls sends an email to 2 given recipient with global personalization.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1"},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to 2 given recipient with global personalization.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!",
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
This calls sends an email to 2 given recipient with global personalization.
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!",
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
      /// This calls sends an email to 2 given recipient with global personalization.
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you!")
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
     * This calls sends an email to 2 recipients with global and contact personalization.
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day:\"monday\"]], may the delivery force be with you! [[var:personalmessage:\"\"]]")
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
This calls sends an email to 2 recipients with global and contact personalization.
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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]',
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
This calls sends an email to 2 recipients with global and contact personalization.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
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
  html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
  vars: {'day' => 'Monday'},
  recipients: [
    {'Email' => 'passenger1@mailjet.com', 'Name' => 'passenger 1', 'Vars' => {'day' => 'Tuesday', 'personalmessage' => 'Happy birthday!'}},
    {'Email' => 'passenger2@mailjet.com', 'Name' => 'passenger 2'}]
)
```
```shell
# This calls sends an email to 2 recipients with global and contact personalization.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
		"Vars":{"day":"Monday"},
		"Recipients":[{"Email":"passenger1@mailjet.com","Name":"passenger 1","Vars":{"day":"Tuesday","personalmessage":"Happy birthday!"}},{"Email":"passenger2@mailjet.com","Name":"passenger 2"}]
	}'
```
```javascript
/**
 *
 * This calls sends an email to 2 recipients with global and contact personalization.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
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
This calls sends an email to 2 recipients with global and contact personalization.
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]",
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
      /// This calls sends an email to 2 recipients with global and contact personalization.
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this [[var:day]], may the delivery force be with you! [[var:personalmessage]]")
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


To go further in personalization <code>Vars</code> is also available for each recipient. The main <code>Vars</code> will be overidden by the recipient <code>Vars</code>


<div></div>
<h3 id="using-contact-properties-v3">Use contact properties</h3>

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
						.property(Email.HTMLPART, "<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!")
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
	"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
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
  html_part: '<h3>Dear [[data:firstname: "passenger"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!',
  recipients:[
    {'Email':'passenger1@mailjet.com','Name':'passenger 1'},
    {'Email':'passenger2@mailjet.com','Name':'passenger 2'}])
```
```shell
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
		"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
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
		"Html-part":"<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear [[data:firstname:\"passenger\"]], welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> May the delivery force be with you!")
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


If the contact you are sending an email to is already in Mailjet system with some contact datas, you can leverage this information to personalize your email.

Use <code>[[data:METADATA_NAME]]</code> or <code>[[data:METADATA_NAME:DEFAULT_VALUE]]</code> to insert data in your content.

<code>DEFAULT_VALUE</code> is the default value that will be used if no data is found.

Refer to the [Personalization section](/guides/#personalization-add-contact-properties) for more information on how to add contact properties.

<div id="go-further-with-personalisation"></div>

### Go further with personalization

Mailjet offers a Templating language that can extend the personalization of your transactional messages.
Visit our [Transactional templating](/guides/#transactional-templating) guide to learn about additional substitutions, modification functions and conditional statements you can use to personalize your messages.

<h2 id="using-a-template-v3">Use a Template</h2>

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
     * This call sends a message based on a template.
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
This call sends a message based on a template.
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
```shell
# This call sends a message based on a template.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/send \
	-H 'Content-Type: application/json' \
	-d '{
		"FromEmail":"pilot@mailjet.com",
		"FromName":"Mailjet Pilot",
		"Subject":"Your email flight plan!",
		"Mj-TemplateID":"1",
		"Mj-TemplateLanguage":true,
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
 * This call sends a message based on a template.
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
		"Mj-TemplateID":"1",
		"Mj-TemplateLanguage":"true",
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
# This call sends a message based on a template.
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
This call sends a message based on a template.
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
This call sends a message based on a template.
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
      /// This call sends a message based on a template.
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

You can either create the templates through our online drag and drop tool [Passport](https://www.mailjet.com/feature/passport/) or through the [/template](/reference/email/templates/) resource.

You can learn more about storing your templates through API [here](/guides/#storing-a-template).

You can also follow our [Step by Step guide](/template-language/passport/) to create your first Passport template with templating language.

In the templates, you will be able to use simple [personalization](/guides/#personalization) (<code>[[data:property_name]]</code> or <code>[[var:variable_name]]</code>) or advanced [templating language](#template-api) (<code>{{data:property_name}}</code>, <code>{{var:variable_name}}</code>, conditional statements and loop statements)

In this sample, <code>Mj-TemplateID</code> will be the ID provided by Passport at the end of your designing process or the ID returned by the <code>/template</code> resource.

<aside class="notice">
The <code>Mj-TemplateLanguage</code> property in the payload provided to Send API is optional but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

<h2 id="using-templating-language-v3">Use Template Language</h2>

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
            .property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
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
    'Html-part' => '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}',
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
```shell
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
		"Mj-TemplateLanguage":true,
		"Text-part":"Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
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
		"Mj-TemplateLanguage":"true",
		"Text-part":"Dear passenger, welcome to Mailjet! On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
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
  html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:"monday"}}, may the delivery force be with you! {{var:personalmessage:""}}',
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
    HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br /> On this {{var:day:\"monday\"}}, may the delivery force be with you! {{var:personalmessage:\"\"}}")
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
The <code>Mj-TemplateLanguage</code> property in the payload provided to Send API is optional but if you want to have the templating language interpreted, it will be mandatory and must have a  <code>true</code> value.
</aside>

Find our dedicated guide [here](/template-language/).

<h2 id="adding-email-headers-v3">Add Email Headers </h2>

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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
	"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
  html_part: '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
  headers: {'Reply-To' => 'copilot@mailjet.com'},
  recipients:[{'Email' => 'passenger@mailjet.com'}])
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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

<h2 id="tagging-email-messages-v3">Tag Email Messages</h2>

Mailjet provides 2 properties to tag messages with your own custom information.

These custom pieces of information are returned back in the events you registered to via our [Event API](/guides/#events) and in the messages processed via our [Parse API](/guides/#customid-and-payload).

<div></div>
<h3 id="sending-an-email-with-a-custom-id-v3">Send an email with a custom ID</h3>

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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
	"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
  html_part: '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
  recipients:[{'Email' => 'passenger@mailjet.com'}],
  'Mj-CustomID' => 'PassengerEticket1234')
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
$response = $mj->get(Resources::$Message, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : API Key Statistical campaign/message data.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message?CustomID=PassengerEticket1234
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Message.all(custom_id: "PassengerEticket1234")
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
	. "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
	"os"
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
            Console.WriteLine(string.Format("ErrorMessage: {0}\n", response.GetErrorMessage()));
         }
      }
   }
}
```


From then, your <code>CustomID</code> is linked to our own Message ID. You can also retrieve the message later by providing it to the <code>/message</code> resource <code>CustomID</code> filter.

<div></div>
<h3 id="sending-an-email-with-a-payload-v3">Send an email with a payload</h3>

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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
	"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
  html_part: '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
  recipients:[{'Email' => 'passenger@mailjet.com'}],
  'Mj-EventPayLoad' => 'Eticket,1234,row,15,seat,B')
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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

<h2 id="grouping-into-a-campaign-v3">Group into a campaign</h2>

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
  'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		recipients: [{'Email'=> 'passenger@mailjet.com'}],
		'Mj-campaign' => 'SendAPI_campaign',
		'Mj-duplicatecampaign' => 1)
```
```shell
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
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

<h2 id="real-time-monitoring-v3">Real-time Monitoring</h2>

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
	'Html-part': '<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!',
	'Recipients': [{'Email':'passenger@mailjet.com'}],
	'Mj-MonitoringCategory': 'Category1'
}
result = mailjet.send.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
This calls sends an email to one recipient with Real-time Monitoring.
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
      HTMLPart: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
      MjMonitoringCategory: "Category1"
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
/*
This calls sends an email to one recipient with Real-time Monitoring.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'Text-part' => "Dear passenger, welcome to Mailjet! May the delivery force be with you!",
    'Html-part' => "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
    'Recipients' => [
        [
            'Email' => "passenger@mailjet.com"
        ]
    ],
    'Mj-MonitoringCategory' => "Category1"
];
$response = $mj->post(Resources::$Email, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
``` ruby
# This calls sends an email to one recipient with Real-time Monitoring.
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
		html_part: "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		recipients: [{ 'Email'=> 'passenger@mailjet.com'}],
		'Mj-MonitoringCategory': "Category1")
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
     * This calls sends an email to one recipient with Real-time Monitoring.
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
						.property(Email.HTMLPART, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
						.property(Email.MJMONITORINGCATEGORY, "Category1")
						.property(Email.RECIPIENTS, new JSONArray()
                .put(new JSONObject()
                    .put("Email", "passenger@mailjet.com")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```shell
# This calls sends an email to one recipient with Real-time Monitoring.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-MonitoringCategory": "Category1"
	}'
```
```javascript
/**
 *
 * This calls sends an email to one recipient with Real-time Monitoring.
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
		"Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
		"Recipients":[{"Email":"passenger@mailjet.com"}],
		"Mj-MonitoringCategory": "Category1"
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
      /// This calls sends an email to one recipient with Real-time Monitoring.
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
            .Property(Send.HtmlPart, "<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!")
            .Property(Send.MjMonitoringCategory, "Category1")
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


Real-time Monitoring enables you to have a constant supervision on your transactional messages by creating custom alerts on categories of Messages.

You can create as many alerts as you want under one of our four types of monitoring alerts, allowing you to fully control your sending:

 - Unusual volume: triggers a notification when emails of the chosen category are not sent/delivered at all, or the volume is unusually low over a chosen period of time.
 - Error on critical message: alerts you each time a message is blocked by our systems or bounces from the recipient’s inbox.
 - Delivery delay: notifies you when emails are delivered slower than expected.
 - Unusual statistics: sends you an alert when delivery, bounce or open rate are lower or higher than your predefined threshold.

Mailjet’s Real-time monitoring will inform you by email, Slack or even SMS based on the alert configuration.

After creating your categories and alerts in our [user interface](https://app.mailjet.com/monitoring#overview), you can leverage the <code>Mj-MonitoringCategory</code> property to specify which category the message you are sending is related to.

<h2 id="send-api-json-properties-v3">Send API json properties</h2>

Property Name | Description
  ----------|------------
FromEmail | Must be a valid, activated and registered sender for this account <br />May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />**MANDATORY - MAX FROM: 1**
FromName | Must be a valid, activated and registered sender for this account <br />May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />
Sender | This can be set only on given API Keys. Contact the <a href="https://app.mailjet.com/support/ticket" target="_blank">support team</a> if you want us to enable this setting on your account.<br />Must be a valid active sender for this account.<br />Perform a simple GET on resource <code>/sender</code> to view a list of allowed senders for your account, or within the Mailjet Account Settings under <a href="https://app.mailjet.com/account/sender" target="_blank">Sender Addresses</a><br />**MAX SENDER: 1**
Recipients | List of recipients, Must include at least a property <code>Email</code> in each element<br />Sample: <code>[{"Email":"passenger@mailjet.com","Name":"passenger"}]</code><br />**MANDATORY**
To | May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />If a recipient is specified twice (in the to, cc, or bcc), it is counted only once.<br />Can be a magic list @lists.mailjet.com. See the <code>Address</code> [contactslist](/reference/email/contacts/contact-list/) property.<br />**MAX RECIPIENTS: 50**
Cc, Bcc | May include the name part: <code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />If one recipient is specified twice, count as one only (including to, cc, bcc)<br />**MAX RECIPIENTS: 50**<br /> **Cc and Bcc can't be used in conjunction with the Recipients property**
Subject | Maximum length is 255 chars <br />**MAX SUBJECTS: 1**
Text-part | Provides the Text part of the message<br />Mandatory if the HTML param is not specified<br />**MANDATORY IF NO HTML - MAX PARTS: 1**
Html-part | Provides the HTML part of the message<br />Mandatory if the text param is not specified<br />**MANDATORY IF NO TEXT - MAX PARTS: 1**
Mj-TemplateID | The Template ID or Name to use as this email content. Overrides the HTML/Text parts if any.<br />Equivalent to using the X-MJ-TemplateID header through SMTP.<br />**MANDATORY IF NO HTML/TEXT - MAX TEMPLATEID: 1**
Mj-TemplateLanguage | Activates the template language processing. By default the template language processing is deactivated. Use <code>True</code> to activate.<br />Equivalent to using the X-MJ-TemplateLanguage header through SMTP.<br />[More information](#use-the-template-in-send-api)
MJ-TemplateErrorReporting | Email Address where a carbon copy with error message is sent to.<br />Equivalent to using the X-MJ-TemplateErrorReporting header through SMTP.<br />[More information](../template-language/sendapi/#templates-error-management)
MJ-TemplateErrorDeliver | Define if the message is delivered if an error is discovered in the templating language. By default the delivery is deactivated. Use <code>deliver</code> to let the message be delivered to the recipient, <code>0</code> to stop it. <br />Equivalent to using the X-MJ-TemplateErrorDeliver header through SMTP.<br />[More information](../template-language/sendapi/#templates-error-management)
Attachments | Attach files automatically to this Email<br />Sum of all attachments, including inline may not exceed 15 MB total<br />Sample: [{"Content-type": "MIME TYPE", "Filename": "FILENAME.EXT", "content":"BASE64 ENCODED CONTENT"}]
Inline_attachments | Attach a file for inline use via <code>cid:FILENAME.EXT</code><br />Sum of all attachements, including inline may not exceed 15 MB total<br />Sample: [{"Content-type": "MIME TYPE", "Filename": "FILENAME.EXT", "content":"BASE64 ENCODED CONTENT"}]
Mj-prio | Manage message processing priority inside your account (API key) scheduling queue.<br />Default is <code>2</code> as in the SMTP submission.<br />Equivalent of using <code>X-Mailjet-Prio</code> header through SMTP<br /><a href="https://app.mailjet.com/docs/email-priority-management" target="_blank">More information</a>
Mj-campaign | Groups multiple messages in one campaign<br />Equivalent of using <code>X-Mailjet-Campaign</code> header through SMTP.<br /><a href="https://app.mailjet.com/docs/emails_headers" target="_blank">More information</a>
<span style="white-space: nowrap;">Mj-deduplicatecampaign</span> | Block/unblock messages to be sent multiple times inside one campaign to the same contact.<br>- <code>0</code>: unblocked (default behavior)<br />- <code>1</code>: blocked<br />Equivalent of using <code>X-Mailjet-DeduplicateCampaign</code> header through SMTP.<br />Can only be used if mj-campaign is specified.<br /><a href="#custom-headers" target="_blank">More information</a>
Mj-trackopen | Force or disable open tracking on this message, can override account preferences.<br />Can only be used with a HTML part.<br />- <code>0</code>: take the values defined on the account. The ones shown <a href="https://app.mailjet.com/account/settings" target="_blank">here</a><br />- <code>1</code>: disable the tracking <br />- <code>2</code>: enable the tracking<br />A <code>X-Mailjet-TrackOpen</code> SMTP header is also available to modify the tracking behaviour.<br />[More information on custom SMTP headers](/guides/#custom-headers)
Mj-trackclick | Force or disable click tracking on this message, can override account preferences.<br />Can only be specified if the HTML part is provided.<br />- <code>0</code>: take the values defined on the account. The ones shown <a href="https://app.mailjet.com/account/settings" target="_blank">here</a><br />- <code>1</code>: disable the tracking <br />- <code>2</code>: enable the tracking<br />A <code>X-Mailjet-TrackClick</code> SMTP header is also available to modify the tracking behaviour.<br />[More information on custom SMTP headers](/guides/#custom-headers)
Mj-CustomID | Attach a custom ID to the message<br />Equivalent to using the X-MJ-CustomID header through SMTP.<br />[More information](#sending-an-email-with-a-custom-id)
Mj-EventPayLoad | Attach a payload to the message<br />Equivalent to using the X-MJ-EventPayload header through SMTP.<br />[More information](#sending-an-email-with-a-payload)
Mj-MonitoringCategory | Category for Real-time Monitoring to which the message will be attached<br />Equivalent of using <code>X-MJ-MonitoringCategory</code> header through SMTP.
Headers | Add lines to the email's headers<br />List of headers as <code>"property":"value"</code> pairs<br />Notice: overriding values of header properties (ie: subject)<br />Sample: <code>{"X-My-Header":"my own value","X-My-Header-2":"my own value 2"}</code>
Vars | Global variables used for personalization<br />Equivalent of using <code>X-MJ-Vars</code> header through SMTP.
Messages | List of messages<br /> Used for bulk emailing.<br />[More information](#sending-in-bulk)

<h2 id="send-api-errors-v3">Send API errors</h2>

Error Code | Message | Reason
  ----------|------------|------------
400 | Invalid characters detected | A non UTF-8 character was detected
400 | "Headers" is not JSON object type | The Headers property should follow the format {"X-My-Header":"my own value","X-My-Header-2":"my own value 2"}
400 | Header [Property name] is not string type |
400 | Missing "To" or "Recipients" property | Send API must be called with either "To" or "Recipient" properties
400 | Too many recipients in To <br /> Too many recipients in Cc <br /> Too many recipients in Bcc <br />|  To, Cc and Bcc can't exceed 50 Email addresses

## Send API v3 to v3.1

Send API v3.1 is not backward compatible with v3. Listed below are all the changes in implementation that should be carefully taken into account when migrating from v3 to v3.1.

### Name and format of message property changes

v3 | v3.1 | Comment
----------|------------|------------
FromEmail | From['Email'] |  
FromName | From ['Name'] |
Sender | Sender |  Change of format : used to be boolean. Now, it is an object, containing <code>Email</code> and <code>Name</code> properties.
Recipients | n/a | If you wish to send a message to many recipients, you need to create multiple instances of messages in the <code>Messages</code> property.
To, Cc, Bcc | To, Cc, Bcc | The SMTP style description of the recipients is replaced by a collection of recipients objects (with <code>Email</code> and <code>Name</code> property), nested in array.
Subject | Subject |
Text-part | TextPart |
Html-part | HTMLPart |
Mj-TemplateID | TemplateID | When using Send API v3, <code>Mj-TemplateID</code> accepts 2 formats. </br> If an integer is provided, we will search for the ID of the template. If a string is provided, we will search for the Name of the template. </br> When using Send API v3.1, <code>TemplateID</code> only accepts `integer` values, i.e. the ID of the template.
Mj-TemplateLanguage | TemplateLanguage |
MJ-TemplateErrorReporting | TemplateErrorReporting | Change of the property type. The destination of the error log should not be provided as string, but as object instead. Correct format: <code> {Email, Name} </code>
MJ-TemplateErrorDeliver | TemplateErrorDeliver | Change of the property type. It used to accept <code> deliver </code> or <code> 0 </code> as values. Now, it becomes a boolean propery, which accepts <code> true </code> or <code> false </code>
Attachments | Attachments | <code>Content-type</code> becomes <code>ContentType</code><br /> <code>content</code> becomes <code>Base64Content</code><br />  <code>content</code> becomes <code>Base64Content</code>
Inline_attachments | InlinedAttachments | <code>Content-type</code> becomes <code>ContentType</code><br /> <code>content</code> becomes <code>Base64Content</code><br />  <code>content</code> becomes <code>Base64Content</code>
Mj-prio | Priority |
Mj-campaign | CustomCampaign |
<span style="white-space: nowrap;">Mj-deduplicatecampaign</span> | DeduplicateCampaign | The value is now a boolean.
Mj-trackopen | TrackOpens | Change of format. It now accepts string, which is one of the following 3 values: <code> account_default </code> , <code> disabled </code>, <code>enabled </code>
Mj-trackclick | TrackClicks | Change of format. It now accepts string, which is one of the following 3 values: <code> account_default </code> , <code> disabled </code>, <code>enabled </code>
Mj-CustomID | CustomID |
Mj-EventPayLoad | EventPayload |
Headers | Headers | If you were using <code>Headers</code> to specify a Reply-To address, you should now use the <code>ReplyTo</code> property. No other changes.
Headers['Reply-to'] | ReplyTo | We added a property ReplyTo to allow you to specify more easily the Reply-To address. In V3, you would have had to use the <code>Headers</code> property to specify a Reply-To address. This property is a JSON object (with <code>Email</code> and <code>Name</code> property)
Mj-MonitoringCategory | MonitoringCategory |
Vars | Variables |

### Message encapsulation

> For a single message, v3 JSON payload :

```json
{
   "FromEmail":"pilot@mailjet.com",
   "FromName":"Mailjet Pilot",
   "Subject":"Your email flight plan!",
   "Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
   "Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
   "Recipients":[{"Email":"passenger@mailjet.com"}]
}
```

> is now in v3.1:

```json
{
   "Messages":[
      {
         "From": {
            "Email": "pilot@mailjet.com",
            "Name": "Mailjet Pilot"
          },
          "To": [
             {
                "Email": "passenger@mailjet.com",
                "Name": "passenger"
             }
          ],
          "Subject": "Your email flight plan!",
          "TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
          "HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
      }
   ]
}
```

Send API v3.1 allows only to send messages collection (bulk) no single message like v3 allowed. The message(s) will be encapsulated in a single property <code>Messages</code>  containing a list of messages.

<div></div>

### Deprecation of the Recipients message property


> For a single message, v3 JSON payload :

```json
{
   "FromEmail":"pilot@mailjet.com",
   "FromName":"Mailjet Pilot",
   "Subject":"Your email flight plan!",
   "Text-part":"Dear passenger, welcome to Mailjet! May the delivery force be with you!",
   "Html-part":"<h3>Dear passenger, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!",
   "Recipients":[{"Email":"passenger1@mailjet.com"},{"Email":"passenger2@mailjet.com"}]
}
```

> should now be in v3.1:

```json
{
   "Messages":[
      {
         "From": {
            "Email": "pilot@mailjet.com",
            "Name": "Mailjet Pilot"
          },
          "To": [
             {
                "Email": "passenger1@mailjet.com",
                "Name": "passenger1"
             }
          ],
          "Subject": "Your email flight plan!",
          "TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
          "HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
      },
      {
         "From": {
            "Email": "pilot@mailjet.com",
            "Name": "Mailjet Pilot"
          },
          "To": [
             {
                "Email": "passenger2@mailjet.com",
                "Name": "passenger2"
             }
          ],
          "Subject": "Your email flight plan!",
          "TextPart": "Dear passenger 1, welcome to Mailjet! May the delivery force be with you!",
          "HTMLPart": "<h3>Dear passenger 1, welcome to <a href=\"https://www.mailjet.com/\">Mailjet</a>!<br />May the delivery force be with you!"
      }
   ]
}
```

The <code>Recipients</code> property of a message object is not available anymore. To achieve the same behavior, multiple recipients receiving independently the same message, you will need to create several <code>Messages</code> objects with a "To" property.

<div></div>

### Limitation size of JSON payload

The number of message objects authorized in the payload for Send API v3 is 100, also limiting the number of recipients (<code>"To"</code>) authorized on one single Send API call. For Send Api v3.1 this limit is set to 50, as specified in its [Message JSON Properties](#message-json-properties) section.
