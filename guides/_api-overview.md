# Overview

## About the Mailjet API

Hello and welcome to the Mailjet Email API!

The Mailjet API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). It has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. All request and response bodies are encoded in [JSON](http://www.json.org/), including errors.

The API is accessed by making HTTPS requests to a specific version endpoint URL. `GET`, `POST`, `PUT`, and `DELETE` methods dictate how you interact with the available objects.

<aside class="notice">
In the Mailjet API, all <code>PUT</code> requests behave like <code>PATCH</code> requests. The update will affect only the specified properties. The other properties of an existing resource will neither be modified, nor deleted. It also means that all non-mandatory properties can be omitted from your payload.
</aside>

Each endpoint has a list of properties and methods you can see in our [API Reference](https://dev.mailjet.com/reference/).

## Authentication

All Email API endpoints requests are authenticated using [HTTPS Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). It requires you to provide a username and a password for each API request.

The username is your API Key and the password is your API Secret Key - you can find them in your [API Key Management page](https://app.mailjet.com/account/api_keys). Both keys are generated automatically when your account is created.

## Pagination

Depending on your request and the endpoint, the results in the response may be paginated. Use the following query parameters to page through the results:

| Name | Type | Description |
| --- | --- | --- |
| `Limit` | integer | The number of results returned per page. The default value is 10, the maximum is 1000. |
| `Offset` | integer | The index of the first object in the page. For example, if you have set a limit of 100 and want to see objects 101 through 200, then `Offset=100` |
| `Sort` | string | Sort the results by a property and select ascending (`ASC`) or descending (`DESC`) order. The default order is ascending. Keep in mind that this is not available for all properties. Example: `Sort=ArrivedAt+DESC` |

## Status Codes

The Mailjet API uses conventional HTTP response codes to indicate the success or failure of an API request. See the full [list of status codes](https://dev.mailjet.com/reference/overview/errors/) for more information.
