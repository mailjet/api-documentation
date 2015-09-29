#Statistics

##Messages

The Mailjet API offers resources to extracts information for every messages you send. You can also filter through the message statistics to view specific metrics for your messages.

### Information about a message

The response payload of a Send API call will provide you with the <code>MessageID</code> of your messages. You can use this <code>MessageID</code> to access information and statistics about the message. 

{{messageMessageID_GET}}

> API response:

{{message_response}}

Perform a GET on <code>[/message](/email-api/v3/message/)</code> to get basic information about a message such as the contact it was sent to, who it was sent by, if there were any attachments and how large the message was.

The <code>StateID</code> property shows the current status the messages is in. To get the full listing of <code>StateId</code> and their meaning, use the <code>[/messagestate](/email-api/v3/messagestate/)</code> resource.


<div></div>

{{messageinformationMessageID_GET}}

> API response:

{{messageinformation_response}}

The <code>[/messageinformation](/email-api/v3/messageinformation/)</code> resource provides a complement of information to the <code>/message</code>.

The <code>ID</code> property in the response is the <code>MessageID</code>. 

<div></div>

{{messagehistory_GET}}

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

{{messagesentstatisticsMessageID_GET}}

> API response:

{{messagesentstatistics_response}}

The <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> resource shows a summary of the statuses and events of the message. 

<div></div>

### Information about campaign messages

{{messageFilteringCampaign_GET}}

When working with campaigns, you can list your messages by using the filter <code>Campaign</code> on <code>[/message](/email-api/v3/message/)</code> resource or <code>CampaignID</code> on <code>[/messagesentstatistics](/email-api/v3/messagesentstatistics/)</code> and <code>[/messageinformation](/email-api/v3/messageinformation/)</code> resources.

If you don't specify any filter on the above resources, the current day messages will be shown. To access the full list, you can use the filter <code>FromTS</code> and <code>ToTS</code> (Unix timestamp)

<div></div>

###Message Statistics

{{messagestatistics_GET}}

> API response:

{{messagestatistics_response}}

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
{{openinformation_GET}}

> API response:

{{openinformation_response}}

The <code>[/openinformation](/email-api/v3/openinformation/)</code> resource shows the log of the open events on your messages during the selected period.

<div></div>

{{clickstatistics_GET}}

> API response:

{{clickstatistics_response}}

The <code>[/clickstatistics](/email-api/v3/clickstatistics/)</code> resource shows the log of the click events on your messages during the selected period.

<div></div>
{{bouncestatistics_GET}}

> API response:

{{bouncestatistics_response}}

The <code>[/bouncestatistics](/email-api/v3/bouncestatistics/)</code> resource shows the log of the bounce events on your messages during the selected period. 

<div></div>

### statistics per event types

{{openstatistics_GET}}

> API response:

{{openstatistics_response}}

The <code>[/openstatistics](/email-api/v3/openstatistics/)</code> resource shows statistics about the opened messages. You can easily see your ratio of opened email compared to the number of processed messages. The number of processed messages include all statuses types (blocked, bounce, spam, sent...)

<div></div>

{{toplinkclicked_GET}}

> API response:

{{toplinkclicked_response}}

The <code>[/toplinkclicked](/email-api/v3/toplinkclicked/)</code> resource shows a ranking of your most clicked url. 

<div></div>

##Resource Statistics

Mailjet captures a number of statistics for each resource, such as the number of messages that were delivered, opened and blocked. Each of the following resource groups the statistics per resource.

By default, these resources will show you statistics on the full history of the account. When <code>FromTS</code> and <code>ToTS</code> filter are available, they will refer to the a campaign start date. Please visit these resource references for more information on available filters.  

<div></div>

{{contactstatistics_GET}}

> API response:

{{contactstatistics_response}}

Available statistic resources:

 - [/contactstatistics](/email-api/v3/contactstatistics/) :  grouped by contacts
 - [/campaignstatistics](/email-api/v3/campaignstatistics/) :  grouped by campaigns
 - [/domainstatistics](/email-api/v3/domainstatistics/) :  grouped by domains
 - [/liststatistics](/email-api/v3/liststatistics/) :  grouped by contact lists
 - [/listrecipientstatistics](/email-api/v3/listrecipientstatistics/) :  grouped by subscription of contact to a list
 - [/senderstatistics](/email-api/v3/senderstatistics/) :  grouped by senders

##API Key Totals

{{apikeytotals_GET}}

> API response:

{{apikeytotals_response}}

Like with the other statistics methods, you can use the <code>[/apikeytotals](/email-api/v3/apikeytotals/)</code> resource to retrieve total counts for the account associated with this API key, such as the number of messages delivered and opened.

