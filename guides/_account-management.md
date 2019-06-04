# Account management

The Mailjet REST API lets you manage multiple API keys. Each API key will have its own dedicated database for contacts, lists, newsletters and statistics. The contacts and lists will not be shared between API keys (including the main API key).

This functionality can be used to create a test or preprod environment and/or separate your mailing use under separate environments (transactional/marketing, per websites...).

Be aware that all messaging under these sub API keys will be accounted under your main account payment plan.

<div id="creating-a-new-api-key"></div>

## Create a new API key
```php
<?php
/*
Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$body = [
    'Name' => "MynewKEY"
];
$response = $mj->post(Resources::$Apikey, ['body' => $body]);
$response->success() && var_dump($response->getData());
?>
```
```shell
# Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
curl -s \
	-X POST \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/apikey \
	-H 'Content-Type: application/json' \
	-d '{
		"Name":"MynewKEY"
	}'
```
```javascript
/**
 *
 * Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.post("apikey")
	.request({
		"Name":"MynewKEY"
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
# Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Apikey.create(name: "MynewKEY"
)
p variable.attributes['Data']
```
```python
"""
Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
data = {
  'Name': 'MynewKEY'
}
result = mailjet.apikey.create(data=data)
print result.status_code
print result.json()
```
``` go
/*
Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
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
	var data []resources.Apikey
	mr := &Request{
	  Resource: "apikey",
	}
	fmr := &FullRequest{
	  Info: mr,
	  Payload: &resources.Apikey {
      Name: "MynewKEY",
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
import com.mailjet.client.resource.Apikey;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Apikey.resource)
			.property(Apikey.NAME, "MynewKEY");
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
      /// Create : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
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
            Resource = Apikey.Resource,
         }
            .Property(Apikey.Name, "MynewKEY");
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


> Response

```json
{
	"Count": 1,
	"Data": [
		{
			"ACL": "",
			"APIKey": "",
			"CreatedAt": "",
			"ID": "",
			"IsActive": "false",
			"IsMaster": "false",
			"Name": "",
			"QuarantineValue": "",
			"Runlevel": "Normal",
			"SecretKey": "",
			"TrackHost": "",
			"UserID": ""
		}
	],
	"Total": 1
}
```


To create a new API key , you just need to do a <code>POST</code> request on the <code>[/apikey](/reference/email/settings/api-key-configuration/#v3_post_apikey)</code>.

The response will contain a new <code>APIKey</code> and <code>SecretKey</code>. Please allow a few seconds before running any additional API requests on this new APIkey, as the full setup process of this new sub-account will take longer than the API call.

<div id="managing-your-api-keys"></div>

## Manage your API keys

<div id="listing"></div>

### List

```php
<?php
/*
View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
*/
require 'vendor/autoload.php';
use \Mailjet\Resources;
$mj = new \Mailjet\Client(getenv('MJ_APIKEY_PUBLIC'), getenv('MJ_APIKEY_PRIVATE'));
$response = $mj->get(Resources::$Apikey);
$response->success() && var_dump($response->getData());
?>
```
```shell
# View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
curl -s \
	-X GET \
	--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
	https://api.mailjet.com/v3/REST/apikey 
```
```ruby
# View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
require 'mailjet'
Mailjet.configure do |config|
  config.api_key = ENV['MJ_APIKEY_PUBLIC']
  config.secret_key = ENV['MJ_APIKEY_PRIVATE']  
end
variable = Mailjet::Apikey.all()
p variable.attributes['Data']
```
```python
"""
View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
"""
from mailjet_rest import Client
import os
api_key = os.environ['MJ_APIKEY_PUBLIC']
api_secret = os.environ['MJ_APIKEY_PRIVATE']
mailjet = Client(auth=(api_key, api_secret))
result = mailjet.apikey.get()
print result.status_code
print result.json()
```
``` go
/*
View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
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
	var data []resources.Apikey
	_, _, err := mailjetClient.List("apikey", &data)
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
import com.mailjet.client.resource.Apikey;
import org.json.JSONArray;
import org.json.JSONObject;
public class MyClass {
    /**
     * View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
     */
    public static void main(String[] args) throws MailjetException, MailjetSocketTimeoutException {
      MailjetClient client;
      MailjetRequest request;
      MailjetResponse response;
      client = new MailjetClient(System.getenv("MJ_APIKEY_PUBLIC"), System.getenv("MJ_APIKEY_PRIVATE"));
      request = new MailjetRequest(Apikey.resource);
      response = client.get(request);
      System.out.println(response.getStatus());
      System.out.println(response.getData());
    }
}
```
```javascript
/**
 *
 * View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
 *
 */
const mailjet = require ('node-mailjet')
	.connect(process.env.MJ_APIKEY_PUBLIC, process.env.MJ_APIKEY_PRIVATE)
const request = mailjet
	.get("apikey")
	.request()
request
	.then((result) => {
		console.log(result.body)
	})
	.catch((err) => {
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
      /// View : Manage your Mailjet API Keys. API keys are used as credentials to access the API and SMTP server.
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
            Resource = Apikey.Resource,
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


To list your API keys , you just need to do a <code>GET</code> request on the <code>[/apikey](/reference/email/settings/api-key-configuration/#v3_get_apikey)</code>.

<div></div>

### Deactivate

You can deactivate an API key with a <code>PUT</code> request with a payload containing the <code>IsActive</code> property.

## Metasender

When using sub API keys, you will need by default to setup all your senders as they are not shared. In case you want to use the same sender or sender domain on all your sub API keys, you can request the validation of a metasender by our support team.

This metasender will be authorized on all your API keys and validate automatically the use of any additional senders matching this metasender.
