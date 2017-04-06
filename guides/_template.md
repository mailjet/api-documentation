<div id="template-api"></div>
<div id="transactional-templating"></div>
#Template API
## Overview

Template API allows for the storage and access to your templates on the Mailjet system. 

Template API will give you access to all templates : Marketing , Transactional , Automation and snippets

By using templates, you will find no need to provide the content of your message at each call to the [Send API](#use-the-template-in-send-api), just use your predefined template. 

Template API can leverage templates created with [Passport](https://www.mailjet.com/feature/passport/), our free responsive email template builder. 

<aside class="notice">
Template API will also allow you to leverage Mailjet template language for your transactional templates. Find more information <a href="/template-language" target="_blank">here</a>.
</aside>

<div></div>
## Storing a template

Store your templates on your Mailjet account by using the `/template` API resource. These templates can also be edited directly in [Passport](https://www.mailjet.com/passport), our WYSIWYG template editor.

### Creating the template
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
```bash
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Template.create(name: "First Template")
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
	. "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
	"os"
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


The <code>[/template](/email-api/v3/template/)</code> resource allows to store your template on the Mailjet system.

To create a template, `POST` on the <code>/template</code> resource specifying a name.

You can them reuse the template at will for your messages by referencing the <code>ID</code> returned when created.

<div></div>

### Adding a content

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
```bash
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Template_detailcontent.create(id: $ID_TEMPLATE, , html_part: "<html><body><p>Hello {{var:name}}</p></body></html>",text_part: "Hello {{var:name}}")
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
	. "github.com/mailjet/mailjet-apiv3-go"
	"github.com/mailjet/mailjet-apiv3-go/resources"
	"os"
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


Now that you have created a template, you can add the content, which can be Text or Html (<code>Text-part</code> or <code>Html-part</code>).
To do so, you can perform a <code>PUT</code> or <code>POST</code> request  on the <code>[/template/$ID/detailcontent](/email-api/v3/template-detailcontent/)</code> resource.
With a <code>POST</code>, if you only give a value to Html-part or Text-part, the other one will be set to empty.
While with a <code>PUT</code>, if you only give a value to Html-part or Text-part, the other one keeps its previous value.

<div></div>

### Leveraging Template Headers

The <code>[/template/$ID/detailcontent](/email-api/v3/template-detailcontent/)</code> resource allow to store a <code>headers</code> property. You can store in this property a JSON object containing default value for some standard SMTP header or Send API properties. 

When using a template in Send API, the processing of the message will take as default the value from the <code>headers</code> property. It will allow you to call Send API without repeating in each API call the same information. If you specify one of those information in your Send API call, you will overwrite the default values of the template.

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
Note: From Septembre 2016, the JSON format of the <code>Headers</code> has been modified. All templates, generated through Passport prior to this change, are not supporting this format. To allow the <code>Headers</code> format to be compliant with Send API, please re-edit and save your templates in Passport.
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


Use the <code>Mj-TemplateID</code> property in your Send API payload to specify the <code>ID</code> of the the template you created.

You must set the <code>Mj-TemplateLanguage</code> property in the payload at <code>true</code> to have the templating language interpreted.

<div></div>

### Using Passport Templates

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
```bash
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
		"Limit":"100"
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Template.all(owner_type: "user",limit: "100")
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
``` go
/*
View : Find your personal templates
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
	var data []resources.Template
	_, _, err := mailjetClient.List("template", &data, Filter("OwnerType", "user"), Filter("Limit", "100"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
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
```bash
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
		"Limit":"100",
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
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Template.all(edit_mode: "tool",limit: "100",owner_type: "user")
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
``` go
/*
View : Find your templates, created in Passport
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
	var data []resources.Template
	_, _, err := mailjetClient.List("template", &data, Filter("EditMode", "tool"), Filter("Limit", "100"), Filter("OwnerType", "user"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


When using <a href="https://app.mailjet.com/templates/transactional" target="_blank">Passport</a>, you have the option to save your work as a template. Next step will be to preview the template and see how it will look like on different devices - mobile phone, tablet and desktop. You can send a test as well to see how the variables will be printed. On the last step, you will see code sample which can be directly used in your system, after specifying the variables.

You can find your templates with a GET on the <code>/template</code> resource then you can use them using the same Send API call as templates created with the API.

<div></div> 









