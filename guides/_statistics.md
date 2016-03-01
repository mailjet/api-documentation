#Statistics

##Messages

The Mailjet API offers resources to extracts information for every messages you send. You can also filter through the message statistics to view specific metrics for your messages.

### Information about a message

The response payload of a Send API call will provide you with the <code>MessageID</code> of your messages. You can use this <code>MessageID</code> to access information and statistics about the message. 

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Message, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Details of a specific Message (e-mail) processed by Mailjet
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message/$ID_MESSAGE 
```
```ruby
# View : Details of a specific Message (e-mail) processed by Mailjet
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Message.find($ID_MESSAGE)
```
```javascript
/**
 *
 * View : Details of a specific Message (e-mail) processed by Mailjet
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("message")
	.id($ID_MESSAGE)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
``` go
/*
View : Details of a specific Message (e-mail) processed by Mailjet
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
	mr := &MailjetRequest{
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Message;
public class MyClass {
    /**
     * View : Details of a specific Message (e-mail) processed by Mailjet
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Message.resource, ID);
      response = client.get(request);
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


Perform a GET on <code>[/message](/email-api/v3/message/)</code> to get basic information about a message such as the contact it was sent to, who it was sent by, if there were any attachments and how large the message was.

The <code>StateID</code> property shows the current status the messages is in. To get the full listing of <code>StateId</code> and their meaning, use the <code>[/messagestate](/email-api/v3/messagestate/)</code> resource.


<div></div>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messageinformation, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : information for a specific message
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messageinformation/$ID_MESSAGE 
```
```javascript
/**
 *
 * View : information for a specific message
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("messageinformation")
	.id($ID_MESSAGE)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# View : information for a specific message
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Messageinformation.find($ID_MESSAGE)
```
```python
"""
View : information for a specific message
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_MESSAGE'
result = mailjet.messageinformation.get(id=id)
print result.status_code
print result.json()
```
``` go
/*
View : information for a specific message
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
	var data []resources.Messageinformation
	mr := &MailjetRequest{
	  Resource: "messageinformation",
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Messageinformation;
public class MyClass {
    /**
     * View : information for a specific message
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Messageinformation.resource, ID);
      response = client.get(request);
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
			"CampaignID": "51",
			"ClickTrackedCount": "0",
			"ContactID": "45",
			"CreatedAt": "2015-07-06T07:10:24Z",
			"ID": "16888509234525280",
			"MessageSize": "20248",
			"OpenTrackedCount": "0",
			"QueuedCount": "0",
			"SendEndAt": "2015-07-06T07:10:24Z",
			"SentCount": "1",
			"SpamAssassinRules": "",
			"SpamAssassinScore": "0"
		}
	],
	"Total": 1
}
```


The <code>[/messageinformation](/email-api/v3/messageinformation/)</code> resource provides a complement of information to the <code>/message</code>.

The <code>ID</code> property in the response is the <code>MessageID</code>. 

<div></div>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messagehistory, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```bash
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
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("messagehistory")
	.id($ID_MESSAGE)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# View : Event history of a message.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Messagehistory.find($ID_MESSAGE)
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
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Messagehistory;
public class MyClass {
    /**
     * View : Event history of a message.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Messagehistory.resource, ID);
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
View : Event history of a message.
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
	var data []resources.Messagehistory
	mr := &MailjetRequest{
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


> API response:

```json
{
    "Count": 2,
    "Data": [
        {
            "Comment": "",
            "EventAt": 1441112238,
            "EventType": "sent",
            "State": "",
            "Useragent": ""
        },
        {
            "Comment": "",
            "EventAt": 1441116490,
            "EventType": "opened",
            "State": "",
            "Useragent": "Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"
        }
    ],
    "Total": 2
}
```

The <code>[/messagehistory](/email-api/v3/messagehistory/)</code> resource in conjonction with the <code>MessageId</code> of your message allows to list the events for a particular message.

This resource provides a polling alternative to [Event API](#event-api-real-time-notifications).

<div></div>

```bash
# View : statuses and events summary for a specific message
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messagesentstatistics/$ID_MESSAGE 
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messagesentstatistics, ['id' => $id]);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * View : statuses and events summary for a specific message
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("messagesentstatistics")
	.id($ID_MESSAGE)
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```python
"""
View : statuses and events summary for a specific message
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$ID_MESSAGE'
result = mailjet.messagesentstatistics.get(id=id)
print result.status_code
print result.json()
```
```ruby
# View : statuses and events summary for a specific message
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Messagesentstatistics.find($ID_MESSAGE)
```
``` go
/*
View : statuses and events summary for a specific message
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
	var data []resources.Messagesentstatistics
	mr := &MailjetRequest{
	  Resource: "messagesentstatistics",
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
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Messagesentstatistics;
public class MyClass {
    /**
     * View : statuses and events summary for a specific message
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Messagesentstatistics.resource, ID);
      response = client.get(request);
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
			"ArrivalTs": "2015-07-06T07:10:26Z",
			"Blocked": "false",
			"Bounce": "false",
			"BounceDate": "",
			"BounceReason": "",
			"CampaignID": "51",
			"Click": "false",
			"CntRecipients": "1",
			"ComplaintDate": "",
			"ContactID": "45",
			"Details": "",
			"FBLSource": "",
			"MessageID": "16888509234525280",
			"Open": "false",
			"Queued": "false",
			"Sent": "false",
			"Spam": "false",
			"StateID": "",
			"StatePermanent": "false",
			"Status": "opened",
			"ToEmail": "passenger@mailjet.com",
			"Unsub": "false"
		}
	],
	"Total": 1
}
```


The <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> resource shows a summary of the statuses and events of the message. 

<div></div>

### Information about campaign messages

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$filters = [
  'Campaign' => '$CAMPAIGN_ID'
];
$response = $mj->get(Resources::$Message, ['filters' => $filters]);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Details of Messages in a campaign
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/message?Campaign=$CAMPAIGN_ID 
```
```ruby
# View : Details of Messages in a campaign
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Message.all(campaign: "$CAMPAIGN_ID")
```
```javascript
/**
 *
 * View : Details of Messages in a campaign
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("message")
	.request({
		"Campaign":"$CAMPAIGN_ID"
	});
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```python
"""
View : Details of Messages in a campaign
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Campaign': '$CAMPAIGN_ID'
}
result = mailjet.message.get(filters=filters)
print result.status_code
print result.json()
```
``` go
/*
View : Details of Messages in a campaign
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
	_, _, err := mailjetClient.List("message", &data, Filter("Campaign", "$CAMPAIGN_ID"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Message;
public class MyClass {
    /**
     * View : Details of Messages in a campaign
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Message.resource)
                  .filter(Message.CAMPAIGN, "$CAMPAIGN_ID");
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```


When working with campaigns, you can list your messages by using the filter <code>Campaign</code> on <code>[/message](/email-api/v3/message/)</code> resource or <code>CampaignID</code> on <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> and <code>[/messageinformation](/email-api/v3/messageinformation/)</code> resources.

If you don't specify any filter on the above resources, the current day messages will be shown. To access the full list, you can use the filter <code>FromTS</code> and <code>ToTS</code> (Unix timestamp)

<div></div>

###Message Statistics

```bash
# View : API key Campaign/Message statistics.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/messagestatistics 
```
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Messagestatistics);
$response->success() && var_dump($response->getData());
?>
```
```javascript
/**
 *
 * View : API key Campaign/Message statistics.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("messagestatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# View : API key Campaign/Message statistics.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Messagestatistics.all()
```
```python
"""
View : API key Campaign/Message statistics.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.messagestatistics.get()
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Messagestatistics;
public class MyClass {
    /**
     * View : API key Campaign/Message statistics.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Messagestatistics.resource);
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
View : API key Campaign/Message statistics.
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
	var data []resources.Messagestatistics
	_, _, err := mailjetClient.List("messagestatistics", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"AverageClickDelay": "48",
			"AverageClickedCount": "1",
			"AverageOpenDelay": "475.3404",
			"AverageOpenedCount": "1.1702",
			"BlockedCount": "20",
			"BouncedCount": "5",
			"CampaignCount": "213",
			"ClickedCount": "3",
			"DeliveredCount": "262",
			"OpenedCount": "94",
			"ProcessedCount": "287",
			"QueuedCount": "0",
			"SpamComplaintCount": "0",
			"TotalClicksCount": "",
			"TotalOpensCount": "",
			"TransactionalCount": "187",
			"UnsubscribedCount": "0"
		}
	],
	"Total": 1
}
```


The <code>[/messagestatistics](/email-api/v3/messagestatistics/)</code> resource aggregates statistics on your selected filter. It is showing a count of each event attached to the messages you filtered on. 

By default if no filter is defined, this resource will aggregate todays messages statistics. You can use the filters <code>FromTS</code> and <code>ToTS</code> (Unix timestamp) to specify the period of extraction.  

Visit the <code>[/messagestatistics](/email-api/v3/messagestatistics/)</code> resource reference for a full list of available filters.

##Event Statistics

The following statistic resources will allow you to view information about the events on your messages. 
By default, they will show the current day statistics. To show more information, we advise you to use the <code>FromTS</code> and <code>ToTS</code> filters to increase the range of extraction. Visit each resource reference for even more filters allowing you to navigate these statistics.  

### list per event types

The following resources will allow you to filter by events. 

To explore the messages with sent event, you can use the <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> resource described above.

<div></div>
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Openinformation);
$response->success() && var_dump($response->getData());
?>
```
```ruby
# View : Retrieve informations about messages opened at least once by their recipients.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Openinformation.all()
```
```bash
# View : Retrieve informations about messages opened at least once by their recipients.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/openinformation 
```
```javascript
/**
 *
 * View : Retrieve informations about messages opened at least once by their recipients.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("openinformation")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```python
"""
View : Retrieve informations about messages opened at least once by their recipients.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.openinformation.get()
print result.status_code
print result.json()
```
``` go
/*
View : Retrieve informations about messages opened at least once by their recipients.
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
	var data []resources.Openinformation
	_, _, err := mailjetClient.List("openinformation", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Openinformation;
public class MyClass {
    /**
     * View : Retrieve informations about messages opened at least once by their recipients.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Openinformation.resource);
      response = client.get(request);
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
			"ArrivedAt": "2015-07-07T07:10:26Z",
			"CampaignID": "51",
			"ContactID": "45",
			"ID": "-1",
			"MessageID": "",
			"OpenedAt": "2015-09-03T11:46:55Z",
			"UserAgentID": "1",
			"UserAgentFull": "Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko Firefox/11.0 (via ggpht.com GoogleImageProxy)"
		}
	],
	"Total": 1
}
```


The <code>[/openinformation](/email-api/v3/openinformation/)</code> resource shows the log of the open events on your messages during the selected period.

<div></div>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Clickstatistics);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Click statistics for messages.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/clickstatistics 
```
```javascript
/**
 *
 * View : Click statistics for messages.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("clickstatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```python
"""
View : Click statistics for messages.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.clickstatistics.get()
print result.status_code
print result.json()
```
```ruby
# View : Click statistics for messages.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Clickstatistics.all()
```
``` go
/*
View : Click statistics for messages.
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
	var data []resources.Clickstatistics
	_, _, err := mailjetClient.List("clickstatistics", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Clickstatistics;
public class MyClass {
    /**
     * View : Click statistics for messages.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Clickstatistics.resource);
      response = client.get(request);
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
			"ClickedAt": "2015-07-07T07:10:28Z",
			"ClickedDelay": "6",
			"ContactID": "45",
			"ID": "-1",
			"MessageID": "16888509234525280",
			"Url": "http://www.example.com",
			"UserAgentID": "692"
		}
	],
	"Total": 1
}
```


The <code>[/clickstatistics](/email-api/v3/clickstatistics/)</code> resource shows the log of the click events on your messages during the selected period.

<div></div>
```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Bouncestatistics);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Statistics on the bounces generated by emails sent on a given API Key.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/bouncestatistics 
```
```ruby
# View : Statistics on the bounces generated by emails sent on a given API Key.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Bouncestatistics.all()
```
```javascript
/**
 *
 * View : Statistics on the bounces generated by emails sent on a given API Key.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("bouncestatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
``` go
/*
View : Statistics on the bounces generated by emails sent on a given API Key.
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
	var data []resources.Bouncestatistics
	_, _, err := mailjetClient.List("bouncestatistics", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```python
"""
View : Statistics on the bounces generated by emails sent on a given API Key.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.bouncestatistics.get()
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Bouncestatistics;
public class MyClass {
    /**
     * View : Statistics on the bounces generated by emails sent on a given API Key.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Bouncestatistics.resource);
      response = client.get(request);
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
			"BouncedAt": "2015-07-07T07:10:28Z",
			"CampaignID": "51",
			"ContactID": "46",
			"ID": "16888509234525280",
			"IsBlocked": "false",
			"IsStatePermanent": "false",
			"StateID": "1"
		}
	],
	"Total": 1
}
```


The <code>[/bouncestatistics](/email-api/v3/bouncestatistics/)</code> resource shows the log of the bounce events on your messages during the selected period. 

<div></div>

### statistics per event types

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Openstatistics);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Retrieve statistics on e-mails opened at least once by their recipients.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/openstatistics 
```
```ruby
# View : Retrieve statistics on e-mails opened at least once by their recipients.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Openstatistics.all()
```
```python
"""
View : Retrieve statistics on e-mails opened at least once by their recipients.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.openstatistics.get()
print result.status_code
print result.json()
```
``` go
/*
View : Retrieve statistics on e-mails opened at least once by their recipients.
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
	var data []resources.Openstatistics
	_, _, err := mailjetClient.List("openstatistics", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```javascript
/**
 *
 * View : Retrieve statistics on e-mails opened at least once by their recipients.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("openstatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Openstatistics;
public class MyClass {
    /**
     * View : Retrieve statistics on e-mails opened at least once by their recipients.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Openstatistics.resource);
      response = client.get(request);
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
			"OpenedCount": "2",
			"OpenedDelay": "7",
			"ProcessedCount": "2"
		}
	],
	"Total": 1
}
```


The <code>[/openstatistics](/email-api/v3/openstatistics/)</code> resource shows statistics about the opened messages. You can easily see your ratio of opened email compared to the number of processed messages. The number of processed messages include all statuses types (blocked, bounce, spam, sent...)

<div></div>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Toplinkclicked);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : Top links clicked historgram.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/toplinkclicked 
```
```javascript
/**
 *
 * View : Top links clicked historgram.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("toplinkclicked")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# View : Top links clicked historgram.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Toplinkclicked.all()
```
``` go
/*
View : Top links clicked historgram.
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
	var data []resources.Toplinkclicked
	_, _, err := mailjetClient.List("toplinkclicked", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```python
"""
View : Top links clicked historgram.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.toplinkclicked.get()
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Toplinkclicked;
public class MyClass {
    /**
     * View : Top links clicked historgram.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Toplinkclicked.resource);
      response = client.get(request);
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
			"ClickedCount": "3",
			"ID": "-1",
			"LinkId": "1",
			"Url": "www.example.com"
		}
	],
	"Total": 1
}
```


The <code>[/toplinkclicked](/email-api/v3/toplinkclicked/)</code> resource shows a ranking of your most clicked url. 

<div></div>

##Resource Statistics

Mailjet captures a number of statistics for each resource, such as the number of messages that were delivered, opened and blocked. Each of the following resource groups the statistics per resource.

By default, these resources will show you statistics on the full history of the account. When <code>FromTS</code> and <code>ToTS</code> filter are available, they will refer to the a campaign start date. Please visit these resource references for more information on available filters.  

<div></div>

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Contactstatistics);
$response->success() && var_dump($response->getData());
?>
```
```bash
# View : View message statistics for a given contact.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contactstatistics 
```
```javascript
/**
 *
 * View : View message statistics for a given contact.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contactstatistics")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```ruby
# View : View message statistics for a given contact.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contactstatistics.all()
```
```python
"""
View : View message statistics for a given contact.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.contactstatistics.get()
print result.status_code
print result.json()
```
``` go
/*
View : View message statistics for a given contact.
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
	var data []resources.Contactstatistics
	_, _, err := mailjetClient.List("contactstatistics", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Contactstatistics;
public class MyClass {
    /**
     * View : View message statistics for a given contact.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Contactstatistics.resource);
      response = client.get(request);
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
			"BlockedCount": "0",
			"BouncedCount": "0",
			"ClickedCount": "0",
			"ContactID": "2",
			"DeliveredCount": "4",
			"LastActivityAt": "2015-09-01T14:08:26Z",
			"MarketingContacts": "0",
			"OpenedCount": "1",
			"ProcessedCount": "4",
			"QueuedCount": "0",
			"SpamComplaintCount": "0",
			"UnsubscribedCount": "0",
			"UserMarketingContacts": "0"
		}
	],
	"Total": 1
}
```


Available statistic resources:

 - [/contactstatistics](/email-api/v3/contactstatistics/) :  grouped by contacts
 - [/campaignstatistics](/email-api/v3/campaignstatistics/) :  grouped by campaigns
 - [/domainstatistics](/email-api/v3/domainstatistics/) :  grouped by domains
 - [/liststatistics](/email-api/v3/liststatistics/) :  grouped by contact lists
 - [/listrecipientstatistics](/email-api/v3/listrecipientstatistics/) :  grouped by subscription of contact to a list
 - [/senderstatistics](/email-api/v3/senderstatistics/) :  grouped by senders

##API Key Totals

```php
<?php
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Apikeytotals);
$response->success() && var_dump($response->getData());
?>
```
```ruby
# View : Global counts for an API Key, since its creation.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Apikeytotals.all()
```
```javascript
/**
 *
 * View : Global counts for an API Key, since its creation.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("apikeytotals")
	.request();
request
	.on('success', function (response, body) {
		console.log (response.statusCode, body);
	})
	.on('error', function (err, response) {
		console.log (response.statusCode, err);
	});
```
```bash
# View : Global counts for an API Key, since its creation.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/apikeytotals 
```
```python
"""
View : Global counts for an API Key, since its creation.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.apikeytotals.get()
print result.status_code
print result.json()
```
```java
package com.my.project;
import com.mailjet.client.errors.MailjetException;
import com.mailjet.client.MailjetClient;
import com.mailjet.client.MailjetRequest;
import com.mailjet.client.MailjetResponse;
import com.mailjet.client.resource.Apikeytotals;
public class MyClass {
    /**
     * View : Global counts for an API Key, since its creation.
     */
    public static void main(String[] args) throws MailjetException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient("api key", "api secret");
      request = new MailjetRequest(Apikeytotals.resource);
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
``` go
/*
View : Global counts for an API Key, since its creation.
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
	var data []resources.Apikeytotals
	_, _, err := mailjetClient.List("apikeytotals", &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


> API response:

```json
{
	"Count": 1,
	"Data": [
		{
			"BlockedCount": "20",
			"BouncedCount": "5",
			"ClickedCount": "3",
			"DeliveredCount": "262",
			"LastActivity": "1441280806",
			"OpenedCount": "94",
			"ProcessedCount": "287",
			"QueuedCount": "0",
			"SpamcomplaintCount": "0",
			"UnsubscribedCount": "0"
		}
	],
	"Total": 1
}
```


Like with the other statistics methods, you can use the <code>[/apikeytotals](/email-api/v3/apikeytotals/)</code> resource to retrieve total counts for the account associated with this API key, such as the number of messages delivered and opened.

