<div id="template-api"></div>
#Transactional templating
## Overview
Leveraging the Mailjet templating language for transactional, designing and building dynamic email, ones that can highly adapt to your recipients profiles, has never been easier. Using a syntax close to the one used by the most popular templating language, such as Jinja2 or Twig, it’s already familiar to you

Go beyond simple personalisation tags by adding logic into your email to include/remove sections of the delivered message.

Additionally, Template API allows for the storage of your templates on the Mailjet system. If you wish, no need to provide the content of your message at each call to the [Send API](#use-the-template-in-send-api), just use your predefined template. 

Finally, Template API can leverage templates created with Passport, our free responsive email template builder - https://www.mailjet.com/feature/passport/

<aside class="warning">
Note: Mailjet Templating language available for transactional messages only.
</aside>

##Reference
### Basic syntax

- [Delimitators](#delimitators)
- [Operators](#operators)
- [Variables](#variables)
- [Functions](#functions)
- [Conditional statements](#conditional-statements)
- [Set function](#set-function)
- [Loop statements](#loop-statements)

### Delimitators
The delimitators are :

 - <code>{% ... %}</code> for Statements
 - <code>{{ ... }}</code> for Expressions to print in the template output

<div></div>

### Operators

Arithmetic Operators :

 - <code>+</code>
 - <code>-</code>
 - <code>*</code>
 - <code>/</code>: float division
 - <code>div</code>: integer division , Format: <code>( X div Y )</code>
 - <code>mod</code>: remainder of division, Format: <code>( X mod Y )</code>

Comparison Operators : 

 - <code>></code>: strict superior
 - <code><</code>: strict inferior
 - <code>>=</code>: superior
 - <code><=</code>: inferior
 - <code>=</code>, <code>==</code>, <code>===</code> equals
 - <code>!=</code>, <code><></code> not equals

Logical operators : 

 - <code>and</code>
 - <code>or</code>
 - <code>xor</code>
 - <code>not</code>

<div></div>
### Variables

Four sources of variables are on offer in the templating languages. They can be used for simple insertion with the <code>{{ ... }}</code> or with operators, functions and control structures. 

<div></div>
#### Vars and Custom Vars

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
      MJTemplateLanguage: "true",
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


You can use the Send API vars and custom vars to push contact specific information to be inserted in the template. This information will not be persistent in Mailjet system and will only be used for the current Send API call. To have persistent properties for a contact, please refer to the next section about <code>ContactData</code>. 

You can then use the syntax <code>{{var:namevar:defaultvalue}}</code> to print the value of the property in your template.

<aside class="notice">
You must set the <code>Mj-TemplateLanguage</code> property in the payload provided to send API at <code>true</code> to have the templating language interpreted. 
</aside>

<div></div>
#### ContactData

In your templates, you can use your contact properties. See the [personalisation guide](#personalisation-add-contact-properties) to learn how to setup and add your own contact properties.

You can then use the syntax <code>{{data:namedata:defaultvalue}}</code> to print the value in your template.

<div></div>
#### Predefined variables

The predefined variables are useful to access information related to the sender, the recipient and the template. They also allow you to access URLs related to your message. 

 - <code>mj:contact</code>: the recipient of the current message. It contains basic information about the contact along with static contact properties under the data object 
  - <code>mj:contact.ID</code> 
  - <code>mj:contact.email</code> 
  - <code>mj:contact.local_part</code> 
  - <code>mj:contact.domain</code> 
  - <code>mj:contact.name</code>	
 - <code>mj:sender</code>: the sender of the current message. It contains basic information about the sender
  - <code>mj:sender.ID</code> 
  - <code>mj:sender.email</code> 
  - <code>mj:sender.local_part</code> 
  - <code>mj:sender.domain</code> 
  - <code>mj:sender.name</code>	
 - <code>mj:template</code>: the template used to generate this message. It contains basic information about the template:
  - <code>mj:template.ID</code> 
  - <code>mj:template.name</code> 
  - <code>mj:template.copyright</code> 
 - <code>mj:unsub_link</code>: the unsubscription link in english (EN)
 - <code>mj:unsub_link_$lang</code>: the unsubscription link in the specified language ([ISO-639-1](https://en.wikipedia.org/wiki/ISO_639-1) formatted)
 - <code>mj:share_twitter</code>: the Twitter link to share the online version of the newsletter
 - <code>mj:share_google</code>: the Google+ link to share the online version of the newsletter
 - <code>mj:share_facebook</code>: the Facebook link to share the online version of the newsletter
 - <code>mj:share_linkedin</code>: the LinkedIn link to share the online version of the newsletter

<div></div>
<h4 id="segmentation_lg">Segmentation</h4>

```html
<html><body>
{% if segment:nameSegment %}
    <h1>Segment nameSegment is true for the recipient</h1>
{% endif %}
</body></html>
```

> Html result

```html
<html><body>
</body></html>
```



The segmentation formulas, you create with <code>[/contactfilter](/email-api/v3/contactfilter/)</code> as described in the [Newsletter Segmentation](#segmentation) section, can be used in the template.
It helps to easily mutualise your newsletter logic and Send API template logic.

You can use either the <code>Name</code> or <code>ID</code> of the <code>/contactfilter</code> in the syntax <code>{{segment:nameORid}}</code> to print the value in your template.

The segmentation variables are also available in condition statement but can't be used and combined with logical operators (AND, OR, NOT, etc). 

<div></div>
### Functions

Behavior of the contact :
 
 - HasOpenedSince(number) : return true if the contact has opened a message in the last number of days
 - HasClickedSince(number) : return true if the contact has clicked on a link in a message in the last number of days

String modification : 

 - Upper(string) : Uppercase the string
 - Lower(string) : Lowercase the string
 - Capitalize(string) : Capitalize the first letter of the string

Mathematical function :
 
 - Round(value, precision) : round a value at precision decimals 
 - Int(value) : Return the integer part of a number

Encoding/ Formating :

 - UrlEncode(string) : Encode a string for use in a URL
 - Escape(html) : 


<div></div>
###Conditional Statements 

```html
<html><body>
{% if data:age > 45 %}
<ul>
    <li>{{data:age}}</li>
</ul>
{% endif %}
</body></html>
```

> Html result

```html
<html><body>
<ul>
    <li>46</li>
</ul>
</body></html>

or

<html><body>
</body></html>
```


> Multiple branches if statement:

```html
{% if data:age >= 21 %}
    You can code, drink and vote!
{% elseif data:age >= 18 %}
    You can't drink but hey, you still can code and vote!
{% else %}
    A few more years to wait !!!
{% endif %}
```

<code>if</code>,<code>else</code>,<code>elseif</code> and <code>endif</code> allow you to insert conditional display of information in your template.

The <code>if</code> statements can be nested inside one another.

The conditional statement allows the usage of a default value for your variables. This can be helpful if you are not always passing all the variables in your Send API call. 

You can specify a default value with the usual format <code>var:name:default_value</code>.

The behavior of the conditional statement will be as follow: 

|                                                  | Var “test” defined <br />and with true value | Var “test” defined <br />and with false value | Var “test” not defined |
|--------------------------------------------------|-----------------------------------------|------------------------------------------|------------------------|
| {% if var:test %}<br />YES<br />{% else %}<br />NO<br />{% endif %}     | YES                                     | NO                                       | !TEMPLATE ERROR!       |
| {% if var:test:true %}<br />YES<br />{% else %}<br />NO<br />{% endif %} | YES                                     | NO                                       | YES                    |
| {% if var:test:false %}<br />YES<br />{% else %}<br />NO<br />{% endif %} | YES                                     | NO                                       | NO                     |
| {{var:test:"N/A"}}                               | true                                    | false                                    | N/A                    |
| {{var:test}}                                     | true                                    | false                                    | !TEMPLATE ERROR!       |

<div></div>

###Set function

> Printing value

```
Hello {% set yo-yo = "Benjamin" %} {{ yo-yo }} !
Hello Benjamin!
```

> You can change the value of the variable anytime. 

```
Hello {% set yo-yo = "Benjamin" %} {{ yo-yo }}, how is {% set yo-yo = "Jane" %} {{ yo-yo }} ?
Hello Benjamin, how is Jane ?
```

> In conditional statement

```
Hello {% set yo-yo = 1 %}{% if yo-yo >= 1 %} Benjamin {% endif %} !
Hello Benjamin!
```

You can use the <code>set</code> function to assign value to a variable, directly in the template. You can then print the value or use it in conditional statements.

You can set values as string, integer, float, boolean, variable or contact data.

<div></div>

###Loop Statements 

> JSON payload 

```json
{
...
"Vars":{"x":[1, true, "Pilot"]}
...
}
```
> or 

```json
{
...
"Vars":{"x":{"test1":1, "test2":true,"test3":"Pilot"}}
...
}
```

> Template

```html
<html><body>
<ul>
{% for value in var:x %}
    <li>{{value}}</li>
{% endfor %}
</ul>
</body></html>
```


> Html result

```html
<html><body>
<ul>
    <li>1</li>
    <li>true</li>
    <li>Pilot</li>
</ul>
</body></html>
```

<code>for in</code> and <code>endfor</code> allows to insert an iteration on a JSON object or array in your template.

The syntax is: 

<code>
<b>{% for</b> variable-name <b>in</b> json-source <b>%}</b><br>
  html or/and nested statement<br>
<b>{% endfor %}</b>
</code>

On each iteration, the value of the current element from <code>json-source</code> is assigned to <code>variable-name</code>. <code>variable-name</code> can be used for display ( ie {{ variable-name }} ) or inside a nested statement.
 
<div></div>

> Multidimensional array 

```json
{
...
"Vars":{ "rows" :[
		{	
			"title":"title1", 
			"bodies":["1","2","3","4"]
		},
		{	
			"title":"title2", 
			"bodies":["5","6"]
		}] 
	}
...
} 

```

> Template

```html
Hello 
	{% for row in var:rows %}loop1:{{ row.title }}
		{% for body in row.bodies %}loop2:{{ body }} {% endfor %}
	{% endfor %}
```


> Html result

```
Hello loop1:title1 loop2:1 loop2:2 loop2:3 loop2:4 loop1:title2 loop2:5 loop2:6
```

> Multiple array 

```json
{
...
"Vars":{ 
	"array1":["1", "2"] , 
	"array2":["3", "4"]
	}
...
} 
```

> Template

```html
Hello 
{% for x3 in var:array1 %} loop1:{{ x3 }}
	{% for x4 in var:array2 %} loop2:{{ x4 }}{% endfor %}
{% endfor %}
```


> Html result

```
Hello  loop1:1 loop2:3 loop2:4 loop1:2 loop2:3 loop2:4
```

The <code>for</code> statements can be nested inside one another. It allows to intersect multiple arrays or objects and manage multidimensional array.

<div></div>
Mailjet templating language support multidimensional json structure:

```json
{
...
"Vars":{ 
	"x":{"test": 2, "test2": { "test3": "Hello" } }
	}
...
} 
```

> Template

```html
{{ var:x.test }} {{ var:x.test2.test3 }}
```

> Html result

```
2 Hello
```

<div></div>
## Storing a template

Instead of providing the template code in each Send API call, you can also store it using the `/template` API resource and re-use it at will in Send API. These templates can be edited directly in [Passport](https://www.mailjet.com/passport), our WYSIWYG template editor.

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
      MJTemplateID: 1,
      MJTemplateLanguage: "true",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
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


Use the <code>Mj-TemplateID</code> property in your Send API payload to specify the <code>ID</code> of the the template you created.

You must set the <code>Mj-TemplateLanguage</code> property in the payload at <code>true</code> to have the templating language interpreted.

<div></div>

### Send a message containing template language

You can pass in the payload of the call to Send API, the usual <code>Text-Part</code> and <code>Html-Part</code> containing the templating language.

Refer to the <code>[Send API](#sending-a-basic-email)</code> guide for more information.

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
```ruby
# View : Find your templates, created in Passport
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Template.all(edit_mode: "tool",limit: "100",owner_type: "user")
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


When using <a href="https://app.mailjet.com/templates/transactional" target="_blank">Passport</a>, you have the option to save your work as a template. Next step will be to preview the template and see how it will look like on different devices - mobile phone, tablet and desktop. You can send a test as well to see how the variables will be printed. On the last step, you will see code sample which can be directly used in your system, after specifying the variables.

You can find your templates with a GET on the <code>/template</code> resource then you can use them using the same Send API call as templates created with the API.

<div></div> 


## Templates error management

```php
<?php
/*
This calls sends a message to a recipient and an template error email.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'FromEmail' => "pilot@mailjet.com",
    'FromName' => "Mailjet Pilot",
    'Subject' => "Your email flight plan!",
    'MJ-TemplateID' => "1",
    'MJ-TemplateLanguage' => true,
    'MJ-TemplateErrorReporting' => "air-traffic-control@mailjet.com",
    'MJ-TemplateErrorDeliver' => "deliver",
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
```bash
# This calls sends a message to a recipient and an template error email.
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
		"MJ-TemplateErrorReporting":"air-traffic-control@mailjet.com",
		"MJ-TemplateErrorDeliver":"deliver",
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
 * This calls sends a message to a recipient and an template error email.
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
		"MJ-TemplateID":"1",
		"Mj-TemplateLanguage":"true",
		"MJ-TemplateErrorReporting":"air-traffic-control@mailjet.com",
		"MJ-TemplateErrorDeliver":"deliver",
		"Recipients":[
				{
						"Email": "passenger@mailjet.com"
				}
		]
	})
request
  .then(result => {
    console.log(result.body)
	})
  .catch(error => {
    console.log(error.statusCode)
	})
```
```ruby
# This calls sends a message to a recipient and an template error email.
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
  "Mj-TemplateErrorReporting": "air-traffic-control@mailjet.com",
  "Mj-TemplateErrorDeliver": "deliver",
  recipients: [{ 'Email'=> 'passenger@mailjet.com'}]
)
```
```python
"""
This calls sends a message to a recipient and an template error email.
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
  'MJ-TemplateErrorReporting': 'air-traffic-control@mailjet.com',
  'MJ-TemplateErrorDeliver': 'deliver',
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
This calls sends a message to a recipient and an template error email.
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
      MJTemplateID: 1,
      MJTemplateLanguage: "true",
      MJTemplateErrorReporting: "air-traffic-control@mailjet.com",
      MJTemplateErrorDeliver: "deliver",
      Recipients: []Recipient {
        Recipient {
          Email: "passenger@mailjet.com",
        },
      },
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
     * This calls sends a message to a recipient and an template error email.
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
						.property(Email.MJTEMPLATEID, "1")
						.property(Email.MJTEMPLATELANGUAGE, true)
						.property(Email.MJTEMPLATEERRORREPORTING, "air-traffic-control@mailjet.com")
						.property(Email.MJTEMPLATEERRORDELIVER, "deliver")
						.property(Email.RECIPIENTS, new JSONArray()
                                                                .put(new JSONObject()
                                                                    .put("Email", "passenger@mailjet.com")));
      response = client.post(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```


Send API offers two payload properties to allow error management:

 - <code>MJ-TemplateErrorReporting</code>: Email Address where a message with the error description is sent to.
 - <code>MJ-TemplateErrorDeliver</code>: Define if the message is delivered to the recipient even if an error is discovered in the templating language.

By default, the delivery of a message in error to the recipient is turned off. 

If an error occurs when the template error reporting is enabled, a message from <code>templating-language-error@mailjet.com</code> with subject <code>Error while evaluating Mailjet Templating language for message "original subject" </code> will be delivered to the given error reporting address. It will contain the error message in the body and an attachement, containing the source of the original message (base64 encoded). 

### Common templating errors:

 - <code> expression parsing error ## Unknown identifier: var:day ## near ## {{var:day ## </code> : This error indicates that the "day" variable is not defined in your <code>Vars</code>. It can be fixed by adding default value for the variable or making sure that you pass all the variables required by the template.
 - <code> expression parsing error ## Unknown identifier: day ## near ## {{day ## </code> : This error is similar to the previous one, except for the absence of namespace (<code>var</code> or <code>data</code>). It can indicates that you forgot to specify whether you want to use a Send API variable <code>var:day</code> or a contact property <code>data:day</code>. It can also indicate that you are trying to use a template variable that is neither define in a <code>set</code> function nor a loop statement.
 - <code> not valid template ## near ## y}}</html> ## </code> : This error occurs when the statement is not finished - missing {% end if %}
 - <code> "var:day" is not an array value </code> : This error is generally returned when you try to loop on a non-array value. 

##Step by step guide


This guide will show you how to quickly be up and running with your first transactional template. This guide leverage Mailjet's responsive mail builder (Passport) which helps you design your emails easily.

At the end of this guide you will have a flexible e-commerce template, which will cover different types of actions - order confirmation (including list of purchased products), out of stock notice, refund request.

OK, let's go! 

 - First, go to the <a href="https://app.mailjet.com/templates/start/transactional" target="_blank">Gallery</a> for transactional templates and pick one. For our demonstration, we chose the one called "Receipt".
 - Configure Subject,  Email Address and Name of the Sender 
 - Design the template
	
In this section, we can actually start designing our own template. You can use templating language as a text, directly added in the template, or you can add it using the button “Template language” which is under the Advanced mode of the builder. You  drag and drop the block on the preferred position in the template.


<a href="images/insert-templ-lang.png" data-featherlight="image">![insert-templ-lang.png](insert-templ-lang.png)</a>


In order to add templating language inside the block, you double click and enter the code on the left hand side of the builder.


<div></div>


You can personalize the name for the recipient using the contact data. 

```html
Dear {{data:firstname:”customer”}}
```

Of course, we would like  our message to vary, depending on the the purpose of the message. We can achieve this by leveraging the conditional statements.


```html
{% if var:step == "confirmorder" %}
    Confirmation of purchase: {{ var:productname:"" }} 

{% elseif var:step == "unavailable" %} 
	Sorry, {{ var:productname:""}} is out of stock :( If you would like a refund, 
	please<a href = "http://mailjet.com/">contact us</a>. 

{% elseif var:step == "refund" %}
	Refund of your purchase {{ var:productname:"" }} has been issued. The amount of 
	{{var:totalprice}} will be returned to your credit card within 10 business days. 

{% endif %}
```

<a href="images/inside_if.png" data-featherlight="image">![/images/inside_if.png](/images/inside_if.png)</a>



<div></div>
<div></div>




But your clients can order N products, how to handle this? Well, let’s use loop structure for that! You can iterate on all the purchased products in order to display them in the template using smart loops.

```html
{% if var:step == "refund" or var:step == "confirmorder" %}
{% for purchase in var:product %}
<ul>PRODUCT TYPE:</ul><li>{{purchase.name}}</li>
<ul>PRODUCT DESCRIPTION:</ul>{{purchase.description}}
{% endfor %}
- Make table with 4 columns in our responsive template builder with column names - Name, Number, Price, Total price. You will print the values nested in the product variable in the 
respective column.
{% for purchase in var:product %}
{{purchase.name}}
{{purchase.number}}
{{purchase.price}}
{{purchase.totalprice}}
{% endfor %}
{% endif %}
```


<a href="images/productlist.png" data-featherlight="image">![/images/productlist.png](/images/productlist.png)</a>



<div></div>
<div></div>




When confirming order, we need to show the customer additional details about the order. We can add another table and print inside the information about the order, stored in the variable <code>order</code>.

```html
{% if var:step == "confirmorder" %}
{{var:totalprice:"Default value"}}
{{var:order_date:"Default value"}}
{{var:order_id:"Default value"}}
{% endif %}
```

<a href="images/orderdetails.png" data-featherlight="image">![/images/orderdetails.png](/images/orderdetails.png)</a>



<div></div>
<div></div>



**And here is what our final resuts will look like!**


<div></div>
<div></div>
####Confirm order:


```json

# API call to send order confirmation
"Vars":
{
"products":[
        {"name":"T-shirt",
        "number":2,
        "price":"$20.00",
        "totalprice":"$40.00",
        "link":"https://s-media-cache-ak0.pinimg.com/736x/25/cf/c1/25cfc1d2d5239d2c37eebdd354d9a285.jpg",
        "description":"<li>Funny T-shirt for coders<\/li> <li>100% cotton<\/li> <li>Machine washable</li>"
        },
        {
        "name":"T-shirt",
        "number":1,
        "price":"$20.00",
        "totalprice":"$20.00",
        "link":"http://rlv.zcache.com/keep_calm_and_code_on_t_shirt_programming_nerd-r7592d3683abe4438863f9192f03bd467_804gy_512.jpg",
        "description":" <li>  Standard fit</li> <li> Fits true to size</li> <li> 5.4 oz. 100% cotton</li> <li>   1x1 rib knit collar and shoulder-to-shoulder taping</li> <li>  Double-needle hem</li>   <li> Imported</li> <li>Machine wash cold</li>"
        }
            ],
"vat":"$12.00",
"totalprice":"$72.00",
"firstname":"Mr. Awesome",
"step":"confirmorder",
"order_date":"10-01-2016",
"order_id":"423482",
"productname":"T-shirt"
}
```


<a href="images/full.png" data-featherlight="image">![/images/full.png](/images/full.png)</a>


<div></div>
<div></div>
####Out of stock:


```json
# API call for out of stock item

"Vars":
{
"products":[
        {"name":"T-shirt",
        "number":2,
        "price":"$20.00",
        "totalprice":"$40.00",
        "link":"https://s-media-cache-ak0.pinimg.com/736x/25/cf/c1/25cfc1d2d5239d2c37eebdd354d9a285.jpg",
        "description":"<li>Funny T-shirt for coders<\/li> <li>100% cotton<\/li> <li>Machine washable</li>"
        },
        {
        "name":"T-shirt",
        "number":1,
        "price":"$20.00",
        "totalprice":"$20.00",
        "link":"http://rlv.zcache.com/keep_calm_and_code_on_t_shirt_programming_nerd-r7592d3683abe4438863f9192f03bd467_804gy_512.jpg",
        "description":" <li>  Standard fit</li> <li> Fits true to size</li> <li> 5.4 oz. 100% cotton</li> <li>   1x1 rib knit collar and shoulder-to-shoulder taping</li> <li>  Double-needle hem</li>   <li> Imported</li> <li>Machine wash cold</li>"
        }
            ],
"vat":"$12.00",
"totalprice":"$72.00",
"firstname":"Mr. Awesome",
"step":"unavailable",
"order_date":"10-01-2016",
"order_id":"423482",
"productname":"T-shirt"
}
```


<a href="images/unavailable.png" data-featherlight="image">![/images/unavailable.png](/images/unavailable.png)</a>


<div></div>
<div></div>
####Refund:



```json

# API call to send refund request

"Vars":
{
"products":[
        {"name":"T-shirt",
        "number":2,
        "price":"$20.00",
        "totalprice":"$40.00",
        "link":"https://s-media-cache-ak0.pinimg.com/736x/25/cf/c1/25cfc1d2d5239d2c37eebdd354d9a285.jpg",
        "description":"<li>Funny T-shirt for coders<\/li> <li>100% cotton<\/li> <li>Machine washable</li>"
        },
        {
        "name":"T-shirt",
        "number":1,
        "price":"$20.00",
        "totalprice":"$20.00",
        "link":"http://rlv.zcache.com/keep_calm_and_code_on_t_shirt_programming_nerd-r7592d3683abe4438863f9192f03bd467_804gy_512.jpg",
        "description":" <li>  Standard fit</li> <li> Fits true to size</li> <li> 5.4 oz. 100% cotton</li> <li>   1x1 rib knit collar and shoulder-to-shoulder taping</li> <li>  Double-needle hem</li>   <li> Imported</li> <li>Machine wash cold</li>"
        }
            ],
"vat":"$12.00",
"totalprice":"$72.00",
"firstname":"Mr. Awesome",
"step":"refund",
"order_date":"10-01-2016",
"order_id":"423482",
"productname":"T-shirt"
}
```


<a href="images/refund.png" data-featherlight="image">![/images/refund.png](/images/refund.png)</a>


<div></div>
<div></div>
##Github Demos

You can find some use case samples on [Github](https://github.com/mailjet/mailjet-apiv3-templating-samples/)

- [All in one transactional](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/all_in_one_transac/) : one template rules all your simple transactional messages. Make it easy to manage a single canvas for all your messages
- [Mailjet Air Electronic ticket](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/electronic_ticket/) : depending on the destination, the seat class and booking of a car to the airport, the layout changes and offers specific call to actions
- [Question / Answers / Comment notifications](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/question_answer/) : alerts for new question, comment and answer (Quandora like) with specific call to action depending on the alerts
- [All in one Ecommerce](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/ecommerce/) : one template to support all your order, payment, shipping... notifications.
- [White label](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/white_label/) : handle several brands with one transactional template
- [RSS to email](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/rss_to_email/) : pull a RSS feed and send it by email (simple loop sample)
- [Product list](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/products_list/) : simple product list (loop sample)
- [Product list in zigzag](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/zigzag_loop/) : product list with alternative image side (loop sample)
- [Ecommerce receipt](https://github.com/mailjet/mailjet-apiv3-templating-samples/tree/master/ecommerce_receipt/) : list of product purchased and total price of purchase (loop sample)







