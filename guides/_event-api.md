# Event API: real-time notifications

The Event API offer a real-time notification through http request on any events related to the messages you sent. The main supported events are <code>open</code>, <code>click</code>, <code>bounce</code>, <code>spam</code>, <code>blocked</code>, <code>unsub</code> and <code>sent</code>. This event notification works for transactional and marketing emails.

The Event API is a very efficient way to do specific actions on your website (log the marketing messages sent to your customers, generate your own statistics, update the unsubscribed contacts on a CRM...). Instead of polling our API a few times a day, we push new data just as the events happen, almost instantly.

## Endpoint URL

```php
<?php
/*
Create a grouped handler for the open event
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'EventType' => "open",
    'Url' => "https://mydomain.com/event_handler",
    'Version' => "2"
];
$response = $mj->post(Resources::$Eventcallbackurl, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create a grouped handler for the open event
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/eventcallbackurl \
	-H 'Content-Type: application/json' \
	-d '{
		"EventType":"open",
		"Url":"https://mydomain.com/event_handler",
		"Version":2
	}'
```
```ruby
# Create a grouped handler for the open event
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Eventcallbackurl.create(event_type: "open",
url: "https://mydomain.com/event_handler",
version: "2"
)
p variable.attributes['Data']
```
```python
"""
Create a grouped handler for the open event
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'EventType': 'open',
  'Url': 'https://mydomain.com/event_handler',
}
result = mailjet.eventcallbackurl.create(data=data)
print result.status_code
print result.json()
```
```javascript
/**
 *
 * Create a grouped handler for the open event
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("eventcallbackurl")
	.request({
		"EventType":"open",
		"Url":"https://mydomain.com/event_handler",
		"Version":2
	})
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
		console.log(err.statusCode)
	})
```
``` go
/*
Create a grouped handler for the open event
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
	var data []resources.Eventcallbackurl
	mr := &Request{
	  Resource: "eventcallbackurl",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Eventcallbackurl {
      EventType: "open",
      Url: "https://mydomain.com/event_handler",
      Version: 2,
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
import com.mailjet.client.resource.Eventcallbackurl;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create a grouped handler for the open event
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Eventcallbackurl.resource)
			.property(Eventcallbackurl.EVENTTYPE, "open")
			.property(Eventcallbackurl.URL, "https://mydomain.com/event_handler")
			.property(Eventcallbackurl.VERSION, "2");
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
      /// Create a grouped handler for the open event
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
            Resource = Eventcallbackurl.Resource,
         }
            .Property(Eventcallbackurl.EventType, "open")
            .Property(Eventcallbackurl.Url, "https://mydomain.com/event_handler")
            .Property(Eventcallbackurl.Version, "2");
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


The endpoint is a URL our server will call for each event (it can lead to a lot of hits !). You can use the API to setup a new endpoint using the <code>[/eventcallbackurl](/reference/email/webhook/)</code> resource. Alternatively, you can configure this in your account preferences, in the <a href="https://app.mailjet.com/account/triggers" target="_blank">Event Tracking</a> section.

It must return a <code>200 OK</code> HTTP code if all goes well. Any other HTTP code will result in our server retrying the request later.

**Our system will retry every 30 seconds and will stop 24 hours after the initial failure, unless a new event is generated.**

The event API also allows you to configure a backup endpoint URL with the property <code>isBackup</code> that will be used in case the primary URL is suspended.

To reactivate a suspended endpoint URL, you need to update the URL with a new URL.

We strongly recommend using a secure (HTTPS) URL in combination with a basic authentication to make sure data cannot be intercepted, and that only our servers can send you data.

E.g.: <code>https://username:password@www.example.com/mailjet_triggers.php</code>

You can also specify a port in your webhook URL.

E.g.: <code>https://www.example.com:123/mailjet_triggers.php</code>

The event data is sent in the <code>POST</code> request body using a JSON object. Its content depends on the event.

<div></div>

> JSON Sample delivered to the webhook URL:

```json
[
   {
      "event": "sent",
      "time": 1433333949,
      "MessageID": 19421777835146490,
      "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
      "email": "api@mailjet.com",
      "mj_campaign_id": 7257,
      "mj_contact_id": 4,
      "customcampaign": "",
      "mj_message_id": "19421777835146490",
      "smtp_reply": "sent (250 2.0.0 OK 1433333948 fa5si855896wjc.199 - gsmtp)",
      "CustomID": "helloworld",
      "Payload": ""
   },
   {
      "event": "sent",
      "time": 1433333949,
      "MessageID": 19421777835146491,
      "Message_GUID": "j2i1hg0-f987-6543-2109-8765e4dc32ba1",
      "email": "api@mailjet.com",
      "mj_campaign_id": 7257,
      "mj_contact_id": 4,
      "customcampaign": "",
      "mj_message_id": "19421777835146491",
      "smtp_reply": "sent (250 2.0.0 OK 1433333948 fa5si855896wjc.199 - gsmtp)",
      "CustomID": "helloworld",
      "Payload": ""
   }
]

```

All the events will be delivered to your webhook in a JSON array of event objects.

Please note that the event types in the collection can be mixed. We group together all the events of the last second for the same webhook URL.

## Best practice

The Event API relies on your server being able to handle large amount of POST calls on your webhook(s).

We advise you to follow some basic guidelines for implementation and usage.

 - Process the payload received asynchronously : as much as possible, the webhook script should rely on an asynchronous consumer process that will use the data saved by your webhook. You should keep out of your webhook logic all cross matches of the delivered events with other resources of our API or your internal database. This step will allow your webhook to answer our calls in a timely manner  and avoid timing out and being retried by our server.
 - Check your server logs regularly for any errors : all non 200 errors would be retried and could cause an increasing volume of calls to your system.
 - Leverage the [transactional message tagging](#tagging-email-messages) to simplify reconciliation between the events and your own system.

## Events

All JSON event objects contain the following properties:

- event : the event type
- time : Unix timestamp of event
- email : email address of recipient triggering the event
- mj_campaign_id : internal Mailjet campaign ID associated to the message
- mj_contact_id : internal Mailjet contact ID
- customcampaign : value of the X-Mailjet-Campaign header when provided
- MessageID : The unique message ID
- Message_GUID : Unique 128-bit ID for this message. Equals the value of MessageUUID returned after the message is sent.
- CustomID: the custom ID, when provided at send time
- Payload: the event payload, when provided at send time

<div></div>

### Sent event

> Sample sent event:

```json
{
   "event": "sent",
   "time": 1433333949,
   "MessageID": 19421777835146490,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "api@mailjet.com",
   "mj_campaign_id": 7257,
   "mj_contact_id": 4,
   "customcampaign": "",
   "mj_message_id": "19421777835146490",
   "smtp_reply": "sent (250 2.0.0 OK 1433333948 fa5si855896wjc.199 - gsmtp)",
   "CustomID": "helloworld",
   "Payload": ""
}
```

Dispatched when the destination SMTP server (gmail, hotmail, yahoo, etc) has accepted the message. Depending on your volume, it could dispatch a lot of events to your system, please make sure you have checked the Group Events Checkbox in the [Event API user interface](https://app.mailjet.com/account/triggers) or that the <code>[/eventcallbackurl](/reference/email/webhook/)</code> <code>version</code> property is set at <code>2</code>

Sent event additional properties:

- mj_message_id : The unique message ID as a string (deprecated, see MessageID)
- smtp_reply: The raw SMTP response message

<div></div>

### Open event
> Sample open event

```json
{
   "event": "open",
   "time": 1433103519,
   "MessageID": 19421777396190490,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "api@mailjet.com",
   "mj_campaign_id": 7173,
   "mj_contact_id": 320,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "ip": "127.0.0.1",
   "geo": "US",
   "agent": "Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0"
}
```

Open event additional properties:

- ip : IP address (can be IPv4 or IPv6) that triggered the event
- geo : country code of IP address (see <a href="http://www.maxmind.com/app/iso3166" target="_blank">list</a>)
- agent : User-Agent

<div></div>

### Click event

> Sample click event

```json
{
   "event": "click",
   "time": 1433334653,
   "MessageID": 19421777836302490,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "api@mailjet.com",
   "mj_campaign_id": 7272,
   "mj_contact_id": 4,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "url": "https://mailjet.com",
   "ip": "127.0.0.1",
   "geo": "FR",
   "agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_0) AppleWebKit/537.36"
}
```

Click event additional properties:

- url : the link that was clicked

<div></div>

### Bounce event

> Sample bounce event

```json
{
   "event": "bounce",
   "time": 1430812195,
   "MessageID": 13792286917004336,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "bounce@mailjet.com",
   "mj_campaign_id": 0,
   "mj_contact_id": 0,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "blocked": false,
   "hard_bounce": true,
   "error_related_to": "recipient",
   "error": "user unknown"
   "comment": "Host or domain name not found. Name service error for name=lbjsnrftlsiuvbsren.com type=A: Host not found"
}
```

Bounce event additional properties:

- blocked : true if this bounce leads to the recipient being blocked
- hard_bounce : true if error was permanent
- error_related_to : see [error table](#possible-values-for-errors)
- error : see [error table](#possible-values-for-errors)
- comment : the raw SMTP error code, including descriptions of the reason for the bounce

<aside class="notice">
NOTICE: If you consider using this event to modify the status of your recipient subscription or viability , please take into account the value of the <code>hard_bounce</code> and <code>error</code> property. All bounce events may not have the same level of importance.
</aside>

<div></div>

### Blocked event

> Sample blocked event

```json
{
   "event": "blocked",
   "time": 1430812195,
   "MessageID": 13792286917004336,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "bounce@mailjet.com",
   "mj_campaign_id": 0,
   "mj_contact_id": 0,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "error_related_to": "recipient",
   "error": "user unknown"
}
```

Blocked event additional properties:

- error_related_to : see error table
- error : see [error table](#possible-values-for-errors)


<aside class="notice">
NOTICE: If you consider using this event to modify the status of your recipient subscription, please take into account the value of the <code>error</code> property. All blocked events may not have the same reason and perpetuity on the status of the contact (i.e. <code>duplicate in campaign</code> indicates that the recipient message was blocked for the campaign and <code>preblocked</code> indicates that the recipient is blocked for all messages).  
</aside>

<div></div>

### Spam event

> Sample spam event

```json

{
   "event": "spam",
   "time": 1430812195,
   "MessageID": 13792286917004336,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "bounce@mailjet.com",
   "mj_campaign_id": 0,
   "mj_contact_id": 0,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "source": "JMRPP"
}
```

Spam event additional properties:

- source : indicates which feedback loop program reported this complaint

<div></div>

### Unsub event

> Sample unsub event

```json
{
   "event": "unsub",
   "time": 1433334941,
   "MessageID": 20547674933128000,
   "Message_GUID": "1ab23cd4-e567-8901-2345-6789f0gh1i2j",
   "email": "api@mailjet.com",
   "mj_campaign_id": 7276,
   "mj_contact_id": 126,
   "customcampaign": "",
   "CustomID": "helloworld",
   "Payload": "",
   "mj_list_id": 1,
   "ip": "127.0.0.1",
   "geo": "FR",
   "agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/42.0.2311.135 Safari/537.36"
}
```
Unsub event additional properties:

- mj_list_id : internal Mailjet List id for REST API access to lists management
- ip : IP address (can be IPv4 or IPv6) that triggered the event
- geo : country code of IP address (see <a href="http://www.maxmind.com/app/iso3166" target="_blank">list</a>)
- agent : User-Agent


## Possible values for errors

error_related_to | error | What really happened ?
-------------- | -------------- | --------------
recipient | user unknown | Email address doesn't exist, double check it for typos !
 | mailbox inactive | Account has been inactive for too long (likely that it doesn't exist anymore).
 | quota exceeded | Even though this is a non-permanent error, most of the time when accounts are over-quota, it means they are inactive.
 | blacklisted | You tried to send to a blacklisted recipient for this account.
 | spam reporter | You tried to send to a recipient that has reported a previous message from this account as spam.
domain | invalid domain | There's a typo in the domain name part of the address. Or the address is so old that its domain has expired !
 | no mail host | Nobody answers when we knock at the door.
 | relay/access denied | The destination mail server is refusing to talk to us.
 | greylisted | This is a temporary error due to possible unrecognized senders. Delivery will be re-attempted.
 | typofix | The domain part of your recipient email address was not valid.
content | bad or empty template | You should check that the template you are using has a content or is not corrupted.
 | error in template language | Your content contains a template language error, you can refer to the [error reporting functionalities](#templates-error-management) to get more information.
spam | sender blocked | This is quite bad! You should contact us to investigate this issue.
 | content blocked | Something in your email has triggered an anti-spam filter and your email was rejected. Please contact us so we can review the email content and report any false positives.
 | policy issue | We do our best to avoid these errors with outbound throttling and following best practices. Although we do receive alerts when this happens, make sure to contact us for further information and a workaround
system | system issue | Something went wrong on our server-side. A temporary error. Please contact us if you receive an event of this type.
 | protocol issue | Something went wrong with our servers. This should not happen, and never be permanent !
 | connection issue | Something went wrong with our servers. This should not happen, and never be permanent !
mailjet | preblocked | You tried to send an email to an address that recently (or repeatedly) bounced. We didn't try to send it to avoid damaging your reputation.
 | duplicate in campaign | You used X-Mailjet-DeduplicateCampaign and sent more than one email to a single recipient. Only the first email was sent; the others were blocked.
