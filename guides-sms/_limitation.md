# Limitations

When using Mailjet's SMS service you may encounter default restrictions of service. Please keep them in mind and feel free to contact us in case you have any questions.

## General API Limitations

 - Mailjet's SMS sending service is purely transactional. __Marketing use of this service is strictly forbidden.__
 - Sending Limit - by default you are limited to sending 200 SMS messages per day (an SMS consisting of multiple concatenated messages is counted as multiple messages). If you try to exceed that limit, you will receive an error message. Please contact our [Support team](https://www.mailjet.com/contact/) or your Customer Success Manager, if you want this limit increased.
 - Encoding - SMS messages that include only standard symbols are encoded in GSM7. If you use any characters that are not supported by the the GSM7 coding standard, the SMS message will be encoded in UNICODE. See [Concatenation & Encoding](#concatenation-and-encoding) for more information.
 - SMS Spoofing - Illegitimate use of [SMS spoofing](https://en.wikipedia.org/wiki/SMS_spoofing) such as impersonating another person, company or product is strictly forbidden.

## Deliverability Limitations

SMS messages sent to countries that are not supported cannot be delivered successfully. Please visit our [Pricing Page](https://www.mailjet.com/pricing/#sms) and use the pricing calculator for more information on supported countries and sending prices.

We review the list of countries regularly and may allow SMS sending to new countries and regions in the future. You can [contact our Support team](https://www.mailjet.com/contact/) to request the opening of a specific country.

## Country Specific Limitations

Different jurisdictions may impose restrictions of their own. Depending on the jurisdiction, one or more of the following may apply:

 - Your Sender ID must be a virtual number.
 - SMS filtering is applied and the Sender ID is modified.
 - Numeric-only Sender IDs are replaced by short codes.
 - You can only send traffic in a limited time window.
