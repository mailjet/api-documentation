# Overview

## About the Mailjet API

Hello and welcome to the Mailjet Email API!

The Mailjet API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). It has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. All request and response bodies are encoded in [JSON](http://www.json.org/), including errors.

The API is accessed by making HTTPS requests to a specific version endpoint URL. `GET`, `POST`, `PUT`, and `DELETE` methods dictate how your interact with the available objects.

<aside class="notice">
In the Mailjet API, all `PUT` requests behave like `PATCH` requests. The update will affect only the specified properties. The other properties of an existing resource will neither be modified, nor deleted. It also means that all non-mandatory properties can be omitted from your payload.
</aside>

Each endpoint has a list of properties and methods you can see in our [API Reference](https://dev.mailjet.com/reference/).

## Authentication

All Email API endpoints requests are authenticated using [HTTPS Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). It requires you to provide a username and a password for each API request.

The username is your API Key and the password is your API Secret Key - you can find them in your [API Key Management page](https://app.mailjet.com/account/api_keys). Both keys are generated automatically when your account is created.

## Pagination

Depending on your request and the endpoint, the results in the response may be paginated. Use the following two query parameters to page through the results:


| Name | Type | Description |
| --- | --- | --- |
| `Limit` | integer | The number of results returned per page. The default value is 10, the maximum is 1000. |
| `Offset` | integer | The index of the first object in the page. For example, if you have set a limit of 100 and want to see objects 101 through 200, then `Offset=100` |

## Status Codes

The Mailjet API uses conventional HTTP response codes to indicate the success or failure of an API request. See the full [list of status](https://dev.mailjet.com/reference/overview/errors/) codes for more information.
