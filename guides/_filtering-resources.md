#Filtering resources

```bash
https://api.mailjet.com/v3/REST/endpoint?filter1=this&filter2=that&filter3=it
```

The Mailjet API provides a set of general filters that can be applied to a <code>GET</code> request for each resource.  In addition to these general filters, each API resource has its own filters that can be used when performing the <code>GET</code>.  You can find these filters in their respective [resource description](/email-api/v3/).

To use a filter in a <code>GET</code>, you can amend the resource URL with a standard query string (<code>?filter1=this&filter2=that&filter3=it</code>).

Mailjet API supports filter combination following these rules: 

 - Only the first occurrence of a filter is taken in account: "?Name=foo&Name=bar&Name=foobar" will only filter on  Name=foo. No error will be returned, all additional occurrences will be skipped. 
 - Combining filter using the query string syntax, "&" results in an AND operator behavior.
 - Some filters support OR and use a "," syntax. Example: MessageStatus on <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> resource accepts MessageStatus=3,4 format. 


##The Limit Filter

```php
<?php
// View : List of 150 contacts
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"Limit" => "150"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : List of 150 contacts
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?Limit=150 
```
```javascript
/**
 *
 * View : List of 150 contacts
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"Limit":"150"
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
# View : List of 150 contacts
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(limit: "150")
```
```python
"""
View : List of 150 contacts
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Limit': '150'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : List of 150 contacts
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("Limit", "150"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


You can limit the number of results by applying the <code>Limit</code> filter. The default value is 10 and the maximum value is 1000. 'Limit=0' delivers the maximum amount of results - 1000.


##The Offset Filter

```php
<?php
// View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"Offset" => "25000"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?Offset=25000 
```
```javascript
/**
 *
 * View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"Offset":"25000"
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
# View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(offset: "25000")
```
```python
"""
View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Offset': '25000'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : List of contact with Offset, delivers 10 contacts, starting with the 25000th contact
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("Offset", "25000"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```

You can use the <code>Offset</code> filter to retrieve a list starting from a certain offset.
<div></div>

```php
<?php
// View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"Limit" => "150",
	"Offset" => "25000"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?Limit=150\&Offset=25000 
```
```javascript
/**
 *
 * View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"Limit":"150",
		"Offset":"25000"
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
# View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(limit: "150",offset: "25000")
```
```python
"""
View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Limit': '150',
  'Offset': '25000'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : List of contacts with Limit and Offset, retrieves a list of 150 contacts starting with the 25000th contact
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("Limit", "150"), Filter("Offset", "25000"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```

The <code>Offset</code> filter can be combined with the <code>Limit</code> filter. 


##The Sort Filter

```php
<?php
// View : List of contact ordered by email in an ascending order
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"Sort" => "email"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : List of contact ordered by email in an ascending order
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?Sort=email 
```
```javascript
/**
 *
 * View : List of contact ordered by email in an ascending order
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"Sort":"email"
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
# View : List of contact ordered by email in an ascending order
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(sort: "email")
```
```python
"""
View : List of contact ordered by email in an ascending order
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Sort': 'email'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : List of contact ordered by email in an ascending order
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("Sort", "email"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


You can sort a GET query by specifying a property name as the value of the 'Sort' filter. The default sorting is ascending.  When a property name is postfixed with <code>+DESC</code> , the sort order is descending.  

Please note that the <code>Sort</code> filter does not work with all properties and that the <code>+DESC</code> is not available for all properties.

<div></div>
```php
<?php
// View : List of contact ordered by email in reverse order
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"Sort" => "email+DESC"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```python
"""
View : List of contact ordered by email in reverse order
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'Sort': 'email+DESC'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : List of contact ordered by email in reverse order
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("Sort", "email+DESC"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```bash
# View : List of contact ordered by email in reverse order
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?Sort=email+DESC 
```
```javascript
/**
 *
 * View : List of contact ordered by email in reverse order
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"Sort":"email+DESC"
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
# View : List of contact ordered by email in reverse order
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(sort: "email+DESC")
```


To retrieve the same query only in descending order, amend the URL with <code>+DESC</code>:

##Resource Filters

```php
<?php
// View : Contacts in ContactsList
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"ContactsList" => "$ContactsListID"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```python
"""
View : Contacts in ContactsList
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'ContactsList': '$ContactsListID'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : Contacts in ContactsList
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("ContactsList", "$ContactsListID"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```bash
# View : Contacts in ContactsList
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?ContactsList=$ContactsListID 
```
```javascript
/**
 *
 * View : Contacts in ContactsList
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"ContactsList":"$ContactsListID"
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
# View : Contacts in ContactsList
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(contacts_list: "$ContactsListID")
```


On each resource, the API provide specific filters. Visit the [reference](/email-api/v3/) to see what is available.

## Unique Key Filters

```php
<?php
// View : Contact from email address
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "VIEW",
	"ID" => "$EMAIL_ADDRESS_OR_CONTACT_ID",
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```python
"""
View : Contact from email address
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$EMAIL_ADDRESS_OR_CONTACT_ID'
result = mailjet.contact.get(id=id)
```
``` go
/*
View : Contact from email address
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
	var data []resources.Contact
	mr := &MailjetRequest{
	  Resource: "contact",
	  ID: RESOURCE_ID,
	}
	err := mailjetClient.Get(mr, &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```bash
# View : Contact from email address
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact/$EMAIL_ADDRESS_OR_CONTACT_ID 
```
```javascript
/**
 *
 * View : Contact from email address
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.id($EMAIL_ADDRESS_OR_CONTACT_ID)
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
# View : Contact from email address
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.find($EMAIL_ADDRESS_OR_CONTACT_ID)
```


You can access each unique element using unique key filter. Visit the [reference](/email-api/v3/) to see the keys available for each resource.

<div></div>
```php
<?php
// View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "VIEW",
	"ID" => "$EventType|$isBackup",
);
$result = $mj->eventcallbackurl($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```python
"""
View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
id = '$EventType|$isBackup'
result = mailjet.eventcallbackurl.get(id=id)
```
``` go
/*
View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
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
	var data []resources.Eventcallbackurl
	mr := &MailjetRequest{
	  Resource: "eventcallbackurl",
	  ID: RESOURCE_ID,
	}
	err := mailjetClient.Get(mr, &data)
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```bash
# View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/eventcallbackurl/$EventType|$isBackup 
```
```javascript
/**
 *
 * View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("eventcallbackurl")
	.id($EventType|$isBackup)
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
# View : event-driven callback URLs, also called webhooks, used by the Mailjet platform when a specific action is triggered
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Eventcallbackurl.find($EventType|$isBackup)
```


In some case the unique key consist of several informations, you can call this unique key combinaison by using the seperator <code>|</code>.

## Like and Case Sensitive Filters

```php
<?php
// View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"NameLike" => "$Name"
);
$result = $mj->liststatistics($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```bash
# View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/liststatistics?NameLike=$Name 
```
```javascript
/**
 *
 * View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("liststatistics")
	.request({
		"NameLike":"$Name"
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
# View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Liststatistics.all(name_like: "$Name")
```
```python
"""
View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'NameLike': '$Name'
}
result = mailjet.liststatistics.get(filters=filters)
```
``` go
/*
View : View Campaign/message/click statistics grouped by ContactsList with Like filter on Name.
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
	var data []resources.Liststatistics
	_, _, err := mailjetClient.List("liststatistics", &data, Filter("NameLike", "$Name"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```


The Mailjet API allows Like and Case Sensitive filtering on selected properties. Visit the [reference](/email-api/v3/) to see if a filter allows this functionality. 

This fonctionality works by adding predefined keywords at the end of a filter: 

 - <code>CI</code> : case insensitive filter
 - <code>Like</code> : like filter similar to a <code>%String%</code> 
 - <code>LikeCI</code> : case insensitive like filter  

Example : <code>Name</code> filter on <code>[/contact/liststatistics](/email-api/v3/liststatistics/)</code> resource. You can use <code>Name</code>, <code>NameCI</code>, <code>NameLike</code> and <code>NameLikeCI</code>.

##The Count Filter

```php
<?php
// View : Total number of contact
$mj = new Mailjet($MJ_APIKEY_PUBLIC,$MJ_APIKEY_PRIVATE);
$params = array(
	"method" => "GET",
	"countOnly" => "1"
);
$result = $mj->contact($params);
if ($mj->_response_code == 200){
   echo "success";
   var_dump($result);
} else {
   echo "error - ".$mj->_response_code;
   var_dump($mj->_response);
}
?>
```
```python
"""
View : Total number of contact
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
filters = {
  'countOnly': '1'
}
result = mailjet.contact.get(filters=filters)
```
``` go
/*
View : Total number of contact
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
	var data []resources.Contact
	_, _, err := mailjetClient.List("contact", &data, Filter("countOnly", "1"))
	if err != nil {
	  fmt.Println(err)
	}
	fmt.Printf("Data array: %+v\n", data)
}
```
```bash
# View : Total number of contact
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/contact?countOnly=1 
```
```javascript
/**
 *
 * View : Total number of contact
 *
 */
var mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
var request = mailjet
	.get("contact")
	.request({
		"countOnly":"1"
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
# View : Total number of contact
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']
  config.default_from = 'your default sending address'
end
variable = Mailjet::Contact.all(count_only: "1")
```


> API response:

```json
{
	"Count": 152,
	"Data": [ ],
	"Total": 152
}
```

Use the filter <code>countOnly</code> to retrieve the number of records a resource will return. This filter will not extract any list of results but only count them. 

<aside class="notice">When you call a resource without the filter <code>countOnly</code>, <code>Count</code> and <code>Total</code> will only show you the number of elements extracted and not the global number.</aside>

##Navigation through results

To navigate on a full set of results, we advise you to either use the filter <code>countOnly</code> to know how many pages of results you will need to extract or to simply loop with a change of <code>Offset</code> until you reach an empty set of results. 





