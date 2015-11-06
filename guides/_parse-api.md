<div id="receive-emails"></div>
#Parse API: Process inbound emails

The Parse API allows you to have emails parsed and its contents delivered to a webhook of your choice.

##Basic Setup

```php
<?php
// Create : ParseRoute description
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Url" => "https://www.mydomain.com/mj_parse.php"
);
$result = $mj->parseroute($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("parseroute")
	.request({
		"Url":"https://www.mydomain.com/mj_parse.php"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : ParseRoute description
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Parseroute.create(url: "https://www.mydomain.com/mj_parse.php")
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
```
``` go
/*
Create : ParseRoute description
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
	var data []resources.Parseroute
	mr := &MailjetRequest{
	  Resource: "parseroute",
	}
	fmr := &FullMailjetRequest{
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


In order to begin receiving emails to your webhook, create a new instance of the Parse API via a <code>POST</code> request on the <code>[/parseroute](/email-api/v3/parseroute/)</code> resource. This call has only one mandatory property - the <code>Url</code> of the webhook (Note: URLs provided cannot be the root).  

The response will provide an <code>Email</code> address you can begin using immediately. It can be used as a Reply-to Email address for example.


##What is delivered to your webhook

```json
{
   "Sender":"miss@mailjet.com",
   "Recipient":"",
   "Date":"20150410T160638",
   "From":"Miss Mailjet <miss@mailjet.com>",
   "Subject":"Hey! It's Friday!",
   "Headers":{  
      "Return-Path":"<miss@mailjet.com>",
      "Received":"by 10.107.134.160 with HTTP; Fri, 10 Apr 2015 09:06:38 -0700 (PDT)",
      "DKIM-Signature":"v=1; a=rsa-sha256; c=relaxed/relaxed;        d=mailjet.com; s=google;        h=mime-version:date:message-id:subject:from:to:content-type;        bh=tsc4ruu5r5loLtAFUwhFp8BIbKzV0AYljT0+Bb/QwWI=;        b=EMkUxZFdmVax53yBCOR4j7pbvv31c0L6+xQ8U/RbHaSH2/VnC2qY6ji6iOhezfoiYy         qpX2fRXCrVNa1tPSU077WMz6z782Fgm3QnONKgWJtVCLrkMtOfZHn+ahNpU3l8I7gSsb         OfWN4cOpYkJUijLfllFPY3p+337rPmZIKsrSA=",
      "X-Google-DKIM-Signature":"v=1; a=rsa-sha256; c=relaxed/relaxed;        d=1e100.net; s=20130820;        h=x-gm-message-state:mime-version:date:message-id:subject:from:to         :content-type;        bh=tsc4ruu5r5loLtAFUwhFp8BIbKzV0AYljT0+Bb/QwWI=;        b=AYNmg8LhUL1+oGQYBvV0yhE2DsNmaMA416enZAvSeQWQir6GfFMbe0N5eDVZ9/QmQK         cguylB8zG+UkJj6liZpfQkOnWZYfjaM1/J04yzPpGTo8eptNZaP1r7E1QKWGEBakldkg         1pIt3nlvIETqwBGMZj+vwYqjusuFPrbhKJxjAeGtLCFXHlP6ePFKEAf37sfRtfvxZ0Wl         8IuJqYSwNkSWqSb/0JzYkecKH1CGDhn5BhKs8VLRrKa/VoNmaEsq7wewpq6PrFCbsv0z         Xf+sl3LXJM5grybzGP+7p2PrbWruYGAdmxF6o38fqjMMCymKC9faQk6ReU/Hlh7J5IGm         Jovw==",
      "X-Gm-Message-State":"ALoCoQlJBEYSiauMbHc8RXQpv3sUJvPmYAd7exYJKZIZFRZtFkSHqDEP59rQK6oIp9mCwPKCirCL",
      "MIME-Version":"1.0",
      "X-Received":"by 10.107.41.72 with SMTP id p69mr3774075iop.58.1428681998638; Fri, 10 Apr 2015 09:06:38 -0700 (PDT)",
      "Date":"Fri, 10 Apr 2015 18:06:38 +0200",
      "Message-ID":"<CAE5Zh0ZpHZ6G5DC+He5426a4RkVab7uWaTDwiMcHzOR=YB3urA@mail.gmail.com>",
      "Subject":"Hey! It's Friday!",
      "From":"Miss Mailjet <miss@mailjet.com>",
      "To":"",
      "Content-Type":"multipart/alternative; boundary=001a1141f3c406f1b2051360f37d"
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

When an email is sent to the email address associated with your instance of the Parse API, the contents of this email will be delivered to your webhook in a JSON format.  Note that the spam score of the email is delivered in the payload via SpamAssassin.

This payload contains a lot of useful information about the message processed. 

This payload is built following a structure where you can parse it and use key information by pointing to them directly (<code>From</code>, <code>Subject</code>, etc.). 

In case you need to loop over every <code>Headers</code> or <code>Parts</code>, you can also use the related collections and loop over it. 

In the <code>Parts</code> collection, the <code>ContentRef</code> property is here to link a specific part (<code>Html</code> or <code>Text</code> for instance) to its associated headers.

Also, note that the <code>CustomID</code> and the <code>Payload</code> properties are returned back if they were provided in the original message sent through the [Send API](#send-transactional-email)

##Manage attachments


In the payload, there is a <code>Parts</code> array. This collection contains every parts of the parsed message. This collection relates directly to how an email is represented in <code>Content multipart</code>. 

If the parsed message contains attachments, they will be also included in the payload. 

To retrieve them, you can either loop on the <code>Parts</code> collection and look for any part where the <code>ContentRef</code> property starts with Attachment or you can look directly for the <code>AttachmentN</code> property (where N is the ID of the attachment in the message, following their order). 

The content of the attachments are always encoded in <a href="http://en.wikipedia.org/wiki/Base64" target="_blank">Base64</a>. 


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

For instance, in a message containing one attachment of type <code>text/plain</code> containing "Hello World!", we will have this payload.

##CustomID and Payload

When using the Send API <code>[Mj-CustomID](#sending-an-email-with-a-custom-id)</code> or <code>[Mj-EventPayLoad](#sending-an-email-with-a-payload)</code>, the Parse API will return the values in the payload under the properties <code>CustomID</code> and <code>Payload</code>. 

CustomId and Payload can be used for example to trace the conversation around your transactional emails.

##Use your own custom domain name


To receive emails on your own domain name, set this domain in your [parseroute](/email-api/v3/parseroute/) instance. Then, add an MX entry on the domain or subdomain DNS to <code>parse.mailjet.com.</code> (final dot is important) and specify your email address based on your domain in the <code>Email</code> attribute.

A less intrusive alternative is to setup a mail forwarding between your current mailbox to the Parse API send-to email automagically provided by Mailjet

```php
<?php
// Create : ParseRoute description
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "POST",
	"Url" => "https://www.mydomain.com/mj_parse.php",
	"Email" => "mjparse@mydomain.com"
);
$result = $mj->parseroute($params);
if ($mj->_response_code == 201){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.post("parseroute")
	.request({
		"Url":"https://www.mydomain.com/mj_parse.php",
		"Email":"mjparse@mydomain.com"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# Create : ParseRoute description
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Parseroute.create(url: "https://www.mydomain.com/mj_parse.php",email: "mjparse@mydomain.com")
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
```
``` go
/*
Create : ParseRoute description
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
	var data []resources.Parseroute
	mr := &MailjetRequest{
	  Resource: "parseroute",
	}
	fmr := &FullMailjetRequest{
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


To use a custom domain name and email address with the Parse API, update your instance via a PUT request with the email you wish to use.



