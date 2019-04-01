<div id="template-api"></div>
<div id="transactional-templating"></div>

# Template API

## Overview

Template API allows for the storage and access to your templates on the Mailjet system.

Template API will give you access to all templates : Marketing , Transactional , Automation and snippets

By using templates, you will find no need to provide the content of your message at each call to the [Send API](#use-the-template-in-send-api), just use your predefined template.

Template API can leverage templates created with [Passport](https://www.mailjet.com/feature/passport/), our free responsive email template builder.

<aside class="notice">
Template API will also allow you to leverage Mailjet template language for your transactional templates. Find more information <a href="/template-language" target="_blank">here</a>.
</aside>

<div id="storing-a-template"></div>
## Store a template

Store your templates on your Mailjet account by using the `/template` API resource. These templates can also be edited directly in [Passport](https://www.mailjet.com/feature/passport), our WYSIWYG template editor.

<div id="creating-the-template"></div>

### Create the template
```php
<?php
/*
Create : 
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Name' => "First Template"
];
$response = $mj->post(Resources::$Template, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : 
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/template \
	-H 'Content-Type: application/json' \
	-d '{
		"Name":"First Template"
	}'
```
```javascript
/**
 *
 * Create : 
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("template")
	.request({
		"Name":"First Template"
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
# Create : 
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Template.create(name: "First Template"
)
p variable.attributes['Data']
```
```python
"""
Create : 
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Name': 'First Template'
}
result = mailjet.template.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : 
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
	var data []resources.Template
	mr := &Request{
	  Resource: "template",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Template {
      Name: "First Template",
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
import com.mailjet.client.resource.Template;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : 
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Template.resource)
			.property(Template.NAME, "First Template");
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
      /// Create : 
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
            Resource = Template.Resource,
         }
            .Property(Template.Name, "First Template");
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
	"Count": 1,
	"Data": [
		{
			"Author": "",
			"Categories": "",
			"Copyright": "",
			"Description": "",
			"EditMode": "",
			"ID": "",
			"IsStarred": "false",
			"Name": "First Template",
			"OwnerId": "5",
			"OwnerType": "",
			"Presets": "",
			"Previews": "",
			"Purposes": ""
		}
	],
	"Total": 1
}
```


The <code>[/template](/reference/email/templates/)</code> resource allows you to store your template in the Mailjet system.

To create a template, `POST` on the <code>/template</code> resource specifying a name.

You can then reuse the template at will for your messages by referencing the <code>ID</code> returned when it's created.

<div id="adding-a-content"></div>

### Add the content

```php
<?php
/*
Create : Template content
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Html-part' => "<html><body><p>Hello {{var:name}}</p></body></html>",
    'Text-part' => "Hello {{var:name}}"
];
$response = $mj->post(Resources::$TemplateDetailcontent, ['id' => $id, 'body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : Template content
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/template/$ID_TEMPLATE/detailcontent \
	-H 'Content-Type: application/json' \
	-d '{
		"Html-part":"<html><body><p>Hello {{var:name}}</p></body></html>",
		"Text-part":"Hello {{var:name}}"
	}'
```
```javascript
/**
 *
 * Create : Template content
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("template")
	.id($ID_TEMPLATE)
	.action("detailcontent")
	.request({
		"Html-part":"<html><body><p>Hello {{var:name}}</p></body></html>",
		"Text-part":"Hello {{var:name}}"
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
# Create : Template content
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Template_detailcontent.create(id: $ID_TEMPLATE, , html_part: "<html><body><p>Hello {{var:name}}</p></body></html>",
text_part: "Hello {{var:name}}"
)
p variable.attributes['Data']
```
```python
"""
Create : Template content
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_TEMPLATE'
data = {
  'Html-part': '<html><body><p>Hello {{var:name}}</p></body></html>',
  'Text-part': 'Hello {{var:name}}'
}
result = mailjet.template_detailcontent.create(id=id, data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Template content
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
	var data []resources.TemplateDetailcontent
	mr := &Request{
	  Resource: "template",
	  ID: RESOURCE_ID,
	  Action: "detailcontent",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.TemplateDetailcontent {
    HtmlPart: "<html><body><p>Hello {{var:name}}</p></body></html>",
    TextPart: "Hello {{var:name}}",
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
import com.mailjet.client.resource.TemplateDetailcontent;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Template content
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(TemplateDetailcontent.resource, ID)
			.property(TemplateDetailcontent.HTMLPART, "<html><body><p>Hello {{var:name}}</p></body></html>")
			.property(TemplateDetailcontent.TEXTPART, "Hello {{var:name}}");
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
      /// Create : Template content
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
            Resource = TemplateDetailcontent.Resource,
            ResourceId = ResourceId.Numeric(ID)
         }
            .Property(TemplateDetailcontent.Htmlpart, "<html><body><p>Hello {{var:name}}</p></body></html>")
            .Property(TemplateDetailcontent.Textpart, "Hello {{var:name}}");
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


Now that you have created a template, you can add the content, which can be Text or Html (<code>Text-part</code> or <code>Html-part</code>).
To do so, you can perform a <code>PUT</code> or <code>POST</code> request on the <code>[/template/$ID/detailcontent](/reference/email/templates/#v3_post_template_template_ID_detailcontent)</code> resource.
With a <code>POST</code>, if you only give a value to Html-part or Text-part, the other one will be set to empty.
While with a <code>PUT</code>, if you only give a value to Html-part or Text-part, the other one keeps its previous value.

<div id="leveraging-template-headers"></div>
### Leverage Template Headers

The <code>[/template/$ID/detailcontent](/reference/email/templates/#v3_post_template_template_ID_detailcontent)</code> resource allows you to store a <code>headers</code> property. In this property you can store a JSON object containing the default value for a standard SMTP header or Send API properties.

When using a template in Send API, the processing of the message will take as default the value from the <code>headers</code> property. It will allow you to call Send API without repeating the same information in each API call. If you specify one of those details in your Send API call, you will overwrite the default values of the template.

<div></div>

> JSON Sample of headers

```json
{
"From":"\"Mailjet pilot\" <pilot@mailjet.com>",
"Subject":"Welcome",
"Reply-to":"copilot@mailjet.com"
}
```

Template headers property | Send API property | Description
  ----------|------------|------------
From | FromName and FromEmail| Sender name and email address<br /><code>john@example.com</code> or <code>&lt;john@example.com&gt;</code> or <code>"John Doe" &lt;john@example.com&gt;</code><br />
Subject | Subject | Message subject (support template language)
Reply-To | Reply-To property of Headers object | Email address that should be used to reply to the message

Templates generated with Passport for transactional will also support the default values listed above.

<aside class="notice">
Note: Since September 2016, the JSON format of the <code>Headers</code> has been modified. All templates, generated through Passport prior to this change, are not supporting this format. To allow the <code>Headers</code> format to be compliant with Send API, please re-edit and save your templates in Passport.
</aside>

<div></div>

## Use the Template in Send API

The templates and templating languages can be used in multiple ways with Send API:

- saving them with the <code>/template</code> resource and using them on demand with the <code>Mj-TemplateID</code> property.
- pushing them at each <code>/send</code> call as a <code>Text-part</code> or <code>Html-part</code>
- use templates you created through the Passport User Interface.

<aside class="notice">
You must set the <code>Mj-TemplateLanguage</code> property in the payload provided to send API at <code>true</code> to have the templating language interpreted.
</aside>

### Send a stored template

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


Use the <code>Mj-TemplateID</code> property in your Send API payload to specify the <code>ID</code> of the the template you created.

You must set the <code>Mj-TemplateLanguage</code> property in the payload at <code>true</code> to have the templating language interpreted.

<div id="using-passport-templates"></div>

### Use Passport Templates

```php
<?php
/*
View : Find your personal templates
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'OwnerType' => 'user',
  'Limit' => '100'
];
$response = $mj->get(Resources::$Template, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : Find your personal templates
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/template?OwnerType=user\&Limit=100 
```
```javascript
/**
 *
 * View : Find your personal templates
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("template")
	.request({
		"OwnerType":"user",
		"Limit":100
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
# View : Find your personal templates
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Template.all(owner_type: "user",
limit: "100"
)
p variable.attributes['Data']
```
```python
"""
View : Find your personal templates
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'OwnerType': 'user',
  'Limit': '100'
}
result = mailjet.template.get(filters=filters)
print result.status_code
print result.json()
```
``` go
/*
View : Find your personal templates
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
	var data []resources.Template
	_, _, err := mailjetClient.List("template", &data, Filter("OwnerType", "user"), Filter("Limit", "100"))
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
import com.mailjet.client.resource.Template;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Find your personal templates
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Template.resource)
                  .filter(Template.OWNERTYPE, "user")
                  .filter(Template.LIMIT, "100");
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
      /// View : Find your personal templates
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
            Resource = Template.Resource,
         }
         .Filter(Template.Ownertype, "user")
         .Filter(Template.Limit, "100");
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

```php
<?php
/*
View : Find your templates, created in Passport
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'EditMode' => 'tool',
  'Limit' => '100',
  'OwnerType' => 'user'
];
$response = $mj->get(Resources::$Template, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : Find your templates, created in Passport
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/template?EditMode=tool\&Limit=100\&OwnerType=user 
```
```javascript
/**
 *
 * View : Find your templates, created in Passport
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("template")
	.request({
		"EditMode":"tool",
		"Limit":100,
		"OwnerType":"user"
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
# View : Find your templates, created in Passport
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Template.all(edit_mode: "tool",
limit: "100",
owner_type: "user"
)
p variable.attributes['Data']
```
```python
"""
View : Find your templates, created in Passport
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'EditMode': 'tool',
  'Limit': '100',
  'OwnerType': 'user'
}
result = mailjet.template.get(filters=filters)
print result.status_code
print result.json()
```
``` go
/*
View : Find your templates, created in Passport
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
	var data []resources.Template
	_, _, err := mailjetClient.List("template", &data, Filter("EditMode", "tool"), Filter("Limit", "100"), Filter("OwnerType", "user"))
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
import com.mailjet.client.resource.Template;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Find your templates, created in Passport
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Template.resource)
                  .filter(Template.EDITMODE, "tool")
                  .filter(Template.LIMIT, "100")
                  .filter(Template.OWNERTYPE, "user");
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
      /// View : Find your templates, created in Passport
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
            Resource = Template.Resource,
         }
         .Filter(Template.Editmode, "tool")
         .Filter(Template.Limit, "100")
         .Filter(Template.Ownertype, "user");
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


When using <a href="https://app.mailjet.com/templates/transactional" target="_blank">Passport</a>, you have the option to save your work as a template. The next step will be to preview the template and see what it will look like on different devices - mobile phone, tablet and desktop. You can send a test as well to see how the variables will be printed. On the last step, you will see code sample which can be directly used in your system, after specifying the variables.

You can find your templates with a GET on the <code>/template</code> resource. Then you can use them using the same Send API call as templates created with the API.

<div></div>
