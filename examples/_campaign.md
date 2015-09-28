#Campaign
##Introduction

A new campaign resource is created for each newsletter and transactional email that is sent.  You can query the campaign resource and its related statistics resources for a variety of data like bounces, number of clicks, and sending time.

##Retrieve general campaign information

In order to get a list of campaigns, perform a GET on the campaign resource.  You may specify a 'FromTS' timestamp.  If no 'FromTS' filter is specified or if 'FromTS' is set to 0, the current timestamp is used.  To get all of the campaigns ever sent, specify a timestamp of 'FromTS=1'.

```bash
curl -X GET --user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
"https://api.mailjet.com/v3/REST/campaign?FromTS=1"
```

```json
{
	"Count": 1,
	"Data": [
		{
			"CampaignType": "",
			"ClickTracked": "",
			"CreatedAt": "",
			"CustomValue": "",
			"FirstMessageID": "",
			"FromID": "",
			"FromEmail": "miss@mailjet.com",
			"FromName": "",
			"HasHtmlCount": "",
			"HasTxtCount": "",
			"ID": "",
			"IsDeleted": "false",
			"IsStarred": "false",
			"ListID": "",
			"NewsLetterID": "",
			"OpenTracked": "",
			"SegmentationID": "",
			"SendEndAt": "",
			"SendStartAt": "",
			"SpamassScore": "",
			"Status": "",
			"Subject": "",
			"UnsubscribeTrackedCount": ""
		}
	],
	"Total": 1
}
```



