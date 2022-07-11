# Filtering Resources

```bash
https://api.mailjet.com/v4/endpoint?filter1=this&filter2=that&filter3=it
```

The Mailjet API provides a set of general filters that can be applied to a <code>GET</code> request for each resource.  In addition to these general filters, each API resource has its own filters that can be used when performing the <code>GET</code>.  You can find these filters in their respective [resource description](/sms-api/v4/sms/).

To use a filter in a <code>GET</code>, you can amend the resource URL with a standard query string (<code>?filter1=this&filter2=that&filter3=it</code>).

Mailjet API supports filter combination following these rules:

 - Only the first occurrence of a filter is taken in account: `?Name=foo&Name=bar&Name=foobar` will only filter on  `Name=foo`. No error will be returned, all additional occurrences will be skipped.
 - Combining filters using the query string syntax, `&` results in an AND operator behavior.
 - Some filters support OR and use a `,` syntax. Example: `StatusCode` on the <code>[/sms](/sms-api/v4/sms/)</code> resource accepts `StatusCode=3,4` format.


## The Limit Filter

```shell
# View : List of 100 SMS messages
curl -s \
	-X GET \
	-H "Authorization: Bearer $MJ_TOKEN" \
	https://api.mailjet.com/v4/sms?Limit=100 
```
```javascript
/**
 *  View : List of 100 SMS messages
 * */
const mailjet = require('node-mailjet')
  .smsConnect(process.env.MJ_API_TOKEN)

const request = mailjet
  .get('sms', { version: 'v4' })
  .request({}, { Limit: 100 })

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```


You can limit the number of results by applying the <code>Limit</code> filter. The default value is 10 and the maximum value is 1000. `Limit=0` delivers the maximum amount of results - 1000.

<div></div>

## The Offset Filter

```shell
# View : List of SMS messages with Offset, starting with the 25000th contact
curl -s \
	-X GET \
	-H "Authorization: Bearer $MJ_TOKEN" \
	https://api.mailjet.com/v4/sms?Offset=25000 
```
```javascript
/**
 *  View : List of SMS messages with Offset, starting with the 25000th contact
 * */
const mailjet = require('node-mailjet')
  .smsConnect(process.env.MJ_API_TOKEN)

const request = mailjet
  .get('sms', { version: 'v4' })
  .request({}, { Offset: 25000 })

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```

You can use the <code>Offset</code> filter to retrieve a list starting from a certain offset.
<div></div>

```shell
# View : List of SMS with Limit and Offset, retrieves a list of 150 SMS starting with the 25000th contact
curl -s \
	-X GET \
	-H "Authorization: Bearer $MJ_TOKEN" \
	https://api.mailjet.com/v4/sms?Limit=150\&Offset=25000 
```
```javascript
/**
 *  View : List of SMS with Limit and Offset, retrieves a list of 150 SMS starting with the 25000th contact
 * */
const mailjet = require('node-mailjet')
  .smsConnect(process.env.MJ_API_TOKEN)

const request = mailjet
  .get('sms', { version: 'v4' })
  .request({}, { Limit: 150, Offset: 25000 })

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```

The <code>Offset</code> filter can be combined with the <code>Limit</code> filter.

<div></div>

## Date Range Filters

```shell
# View : List of contacts sent between March 1st 2018 00:00 UTC and March 10th 2018 00:00 UTC
curl -s \
	-X GET \
	-H "Authorization: Bearer $MJ_TOKEN" \
	https://api.mailjet.com/v4/sms?FromTS=1519862400\&ToTS=1520640000 
```
```javascript
/**
 *  View : List of contacts sent between March 1st 2018 00:00 UTC and March 10th 2018 00:00 UTC
 * */
const mailjet = require('node-mailjet')
  .smsConnect(process.env.MJ_API_TOKEN)

const request = mailjet
  .get('sms', { version: 'v4' })
  .request({}, { FromTS: 1519862400, ToTS: 1520640000 })

request
  .then((result) => {
    console.log(result.body)
  })
  .catch((err) => {
    console.log(err.statusCode)
  })
```

Use the `FromTS` and `ToTS` filters (Unix timestamp) to specify a time period, for which to retrieve the information you need.

<div></div>
