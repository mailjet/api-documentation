#Send marketing campaigns

To send your first newsletter, you need to have at least one active sender address in the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section.

You will need to create contacts and organize them into lists.
To do so, you have two options : use the <a href="https://app.mailjet.com/contacts/lists" target="_blank">interface</a> or the API.

## Create a contact list

{{contactslist_POST}}

> API response:

{{contactslist_response}}

You can create your contacts list by performing a simple <code>POST</code> request on the <code>[/contactslist](/email-api/v3/contactslist/)</code> resource with only one mandatory field : its name.

The <code>Name</code> will be a unique identifier for your list. 

<div></div>

## Manage contacts

### Create a contact

{{contact_POST}} 

> API response:

{{contact_response}}

To create some contacts which will be the recipients, you need to specify an email address with <code>POST</code> on the <code>[/contact](/email-api/v3/contact/)</code> resource. 

The email address will be a unique identifier of your contact in the Mailjet System. 

<div></div>

### Personalisation : add contact properties

The <code>/contact</code> resource allows you to create contacts using their email addresses and names. If you want to add more granular details about your contacts, Mailjet provides the capability to add custom data to contacts. 

The addition of custom data starts with the definition of the extra information to store with the contacts (It could be for example the country the contacts live in, how old the contacts are, their current income, the value of their purchases on your site...) and how this data will be stored (string, number, boolean...). 

#### Defining custom Contact data

{{contactmetadata_POST}}

> API response:

{{contactmetadata_response}}

To define custom contact data, perform a <code>POST</code> on <code>[/contactmetadata](/email-api/v3/contactmetadata/)</code> with the following properties:

 - <code>Name</code>: the name of the custom data field
 - <code>DataType</code>: the type of data that is being stored (this can be either a <code>str</code>, <code>int</code>, <code>float</code> or <code>bool</code>)
 - <code>NameSpace</code>: this can be either <code>static</code> or <code>historic</code>

For example, to store the age of each contacts, a <code>static</code> <code>int</code> "Age" property can be added to the metadata.

A <code>static</code> data stores only one value per DataType. It could store for example a firstname, a lastname, a country, a language ...  
A <code>historic</code> data stores a timestamped history of the value of this data. It could be used for example to store the value of each purchases over the history of the contact. 

These 2 Namespaces have their own resources for viewing, creating and editing: <code>[/contactdata](/email-api/v3/contactdata/)</code> and <code>[/contacthistorydata](/email-api/v3/contacthistorydata/)</code>  

The contact datas will be available for personalisation of your message content and for segmentation of your lists.

<div></div>
#### Adding custom static Contact data

{{contactdata_PUT}}

By Performing a <code>PUT</code> (acting like a PATCH, your other static datas will not be lost) on <code>[/contactdata](/email-api/v3/contactdata/)</code>, you can add values for several metadata at once.

<div></div>
#### Adding custom historic Contact data

{{contacthistorydata_POST}}

By Performing a <code>POST</code> on <code>[/contacthistorydata](/email-api/v3/contacthistorydata/)</code>, you can add a new value for a historic metadata.

This historic metadata will be available for segmentation with dedicated functions like <code>Avg</code>,<code>Min</code>,<code>Max</code>...

<div></div>
#### Creating contacts with Contact data  

{{contactSimple_managemanycontacts_POST}}

Mailjet offers the <code>[/contact/managemanycontacts](/email-api/v3/contact-managemanycontacts/)</code> resource to create or update multiple contacts and their properties at once.
 
[More information](#contact_managemanycontacts)

<div></div>
### Subscribe to a list

{{contact_managecontactslists_POST}}

Last step: you have to add this contact to the contactslist previously created.

<code>POST</code> on the <code>[/contact/$ID/managecontactslists](/email-api/v3/contact-managecontactslists/)</code> with $ID being the Mailjet contact id or the email address of the contact. 
You can specify one or more list to add this contact to. The <code>action</code> specified with the ListID can be <code>addforce</code> (force the subscription of the contact even if he unsubscribed to the list before) or <code>addnoforce</code> (do not resubscribe the contact to the list if unsubscribed before) 

<code>managecontactslist</code> offer more possibilities of list management for a contact. [More information](#managecontactslists)

<div></div>
### Create contact and subscribe at once

{{contactslist_managecontact_POST}}

> API response: 

{{contactslist_managecontact_POST_STATIC_response_STATIC}}

The <code>[/contactslist/$ID/managecontact](/email-api/v3/contactslist-managecontact/)</code> allows to create and add a contact to a list in one call. 

The properties specified in this call can only be static Contact data.

  'Action' is mandatory and can be one of the following values:

  Actions | |
  ----------|------------
  addforce | **string** <br /> adds the contact and resets the unsub status to false
  addnoforce | **string** <br /> adds the contact and does not change the subscription status of the contact

The response will include the <code>ID</code> of the contact that was created or updated. 

## Add Contacts in bulks

###Managing and uploading multiple contacts ( /contact/managemanycontacts ) 

This resource allows to add contacts in bulk in a json format. Optionally, these contacts can be added to existing lists. 
[More information](#contact_managemanycontacts)

###Managing multiple Contacts subscriptions to a List ( /contactslist/$ID/managemanycontacts ) 

This resource allows to add contacts directly to a list. This resource will create new contacts if the contacts are not already in the Mailjet system. 
[More information](#contactslist_managemanycontacts)


###Managing Contacts through CSV upload

This process allows to upload csv containing large quantities of contacts.
[More information](#csv_upload_contacts)

##Prepare a newsletter

### Create a Newsletter

{{newsletter_POST}}

> API response: 

{{newsletter_POST_STATIC_response}}

To create a newsletter, perform a POST on the <code>[/newsletter](/email-api/v3/newsletter/)</code> resource. Required fields are a <code>Locale</code>, <code>Sender</code>, <code>SenderEmail</code>, <code>Subject</code> and <code>ContactsListID</code>.

Reminder: the SenderEmail needs to be active. Visit the <a href="https://app.mailjet.com/account/sender" target="_blank">Sender domains & addresses</a> section to check.

<div></div>
### Add a body to a newsletter

{{newsletter_detailcontent_PUT}}

>API response : 

```json
{
	"Count": 1,
	"Data": [
		{
			"Html-part": "Hello <strong>world</strong>!",
			"Text-part": "Hello world!"
		}
	],
	"Total": 1
}
```

Now that we have a newsletter, we can add the most important property: its content, which can be Text or Html (Text-part or Html-part). To do so, you can perform a <code>PUT</code> or <code>POST</code> request  on the <code>[/newsletter/$ID/detailcontent](/email-api/v3/newsletter-detailcontent/)</code> resource.
With a <code>POST</code>, if you only give a value to Html-part or Text-part, the other one will be set to empty.
While with a <code>PUT</code>, if you only give a value to Html-part or Text-part, the other one keeps its previous value.

## Send a newsletter

### Test Newsletters

{{newsletter_test_POST}}

> API response:

{{newsletter_test_POST_STATIC_response}}

To send a test version of a newsletter, you need to perform a POST request on the resource <code>[/newsletter/$ID/test](/email-api/v3/newsletter-test/)</code> with a recipient in the JSON packet.

<aside class="notice">
Before sending, the API will check if the newsletter has all mandatory fields filled in and that they are valid. In case of error, the API will return a 400 Bad Request.
</aside>

<div></div>

### Send the newsletter immediately

{{newsletter_send_POST}}

> API response:

{{newsletter_send_POST_STATIC_response}}

Once the newsletter is completely set up, it can be sent with a <code>POST</code> on the <code>[/newsletter/$ID/send](/email-api/v3/newsletter-send/)</code> resource.

Before sending, the API will check if the newsletter has all mandatory fields filled in and that they are valid :

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this newsletter


<div></div>

### Schedule a newsletter

> Schedule a newsletter for the 22nd of April, 2015 at 9am UTC+1

{{newsletter_schedule_POST}}

> API response:

{{newsletter_schedule_POST_STATIC_response}}

To send a newsletter at a later date, we need to set the date property (ISO 8601 format) at which the newsletter shall be sent usind the <code>[/newsletter/$ID/schedule](/email-api/v3/newsletter-schedule/)</code> resource.

Before scheduling, the API will check if the newsletter has all mandatory fields filled in and that they are valid:

 - <code>SenderEmail</code> is a valid sender
 - <code>Sender</code> is not empty
 - <code>Subject</code> is not empty
 - <code>ContactsList</code> is an existing contact list with more than one active subscribed recipient
 - <code>Status</code> is <code>Draft</code> or <code>Programmed</code>
 - <code>Text-part</code> and/or <code>Html-part</code> exist
 - <code>AXTesting</code> is not set for this newsletter
 - Date of scheduling is not in the past or too close to the execution

The value <code>NOW</code> is accepted as a date to indicate immediate sending

##Segmentation

Our <code>[/contactfilter](/email-api/v3/contactfilter/)</code> resource allows you to send newsletters to certain subsets of your contact lists. A filter can be applied to any contact metadata that you defined, like age, gender, and location.

###Prerequisites

In order to use a contact filter, you must add additional data to your contacts. Please refer to the [personalisation](#personalisation-add-contact-properties) to see how this is done. You also have to create a contact list, because contact filters are applied to a contact list via the <code>/newsletter</code> resource.

###How does it work?

Mailjet allows you to segment a list of contacts by creating a ContactFilter resource. This resource has a property called expression and it is the value of this property that is used to filter a list of contacts. You can create simple expressions using the <code>=</code>, <code><</code>, <code>></code> and <code>!=</code> operators:

- age>40
- gender=male
- country=France

You can also apply the negative statement <code>not</code>

But you can also combine these operators with the <code>and</code> and <code>or</code> operator, using brackets:

- (age>40) and (gender=male)
- (country=France) and (profession=physician)
- ((age>=50) and (age<70)) or (income>50000)


Additionaly, you can filter on the contact Activities (who has opened/clicked your campaigns in the last X days). These function return a boolean.

- <code>HasOpenedSince(days)</code> 
- <code>HasClickedSince(days)</code> 

These functions are not scoped on a specific campaign. 

<div></div>
###Create a contact filter

{{contactfilter_POST}}

> API response:

{{contactfilter_POST_STATIC_response}}

Lets say that we added an age property to our contacts, using the <code>[/contactmetadata](/email-api/v3/contactmetadata/)</code> and <code>[/contactdata](/email-api/v3/contactdata/)</code> resources. 
We now want to create a filter that gives us only those contacts that are 40 years old. In order to do this, we perform a <code>POST</code> with the following properties: <code>Name</code> , <code>Expression</code> , and <code>Description</code> . Name and Description allow you to describe the filter,while <code>Expression</code> contains the actual filtering expression.

<div></div>
###Create a newsletter with a segmentation filter

{{newsletterSegmentation_POST}}

Segmentation is achieved by adding a contact filter to a newsletter resource using the property <code>SegmentationID</code>. <code>$ID_CONTACT_FILTER</code> is the ID of the contact filter that was created on the previous step.

##Campaign and Statistics

A new campaign resource is created for each newsletter and transactional email that is sent.  You can query the campaign resource and its related statistics resources for a variety of data like bounces, number of clicks, and sending time.

###Retrieve campaign information for a particular newsletter

{{campaignNewsletter_GET}}

> API response:

{{campaign_response}}

When a campaign is created from the processing of a newsletter, its <code>CustomValue</code> property is set to <code>mj.nl=$NEWSLETTER_ID</code> where <code>$NEWSLETTER_ID</code> is the id of the newsletter you just sent. You can use <code>mj.nl=$NEWSLETTER_ID</code> as a unique key to retrieve the campaign.


<div></div>
### Statistics

{{campaignstatistics_GET}}

> API response:

{{campaignstatistics_response}}

View message statistics grouped by campaigns using <code>[/campaignstatistics](/email-api/v3/campaignstatistics/)</code> resource .

The [API reference](/email-api/v3/campaign/) offers more information on additional resources to explore the campaigns.


