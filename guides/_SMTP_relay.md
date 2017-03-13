<h1 id="SMTP_Relay_Use">SMTP Relay</h1>

##Postfix Installation

You can configure your Postfix to send via in.mailjet.com relay, depending on the sender by following these steps: 

In this example, all outgoing emails are sent directly to Mail eXchangers (MX), except when from is *@example.com or example@example.net which are going through Mailjet.

Caution, for Postfix, a sender is not the From: but the sender envelope passed to sendmail (in the 5th mail() argument: -fexample@example.com or in PHP config php.ini : sendmail_path = /path/to/sendmail -t -i -fexample@example.com).

<div></div>
In /etc/postfix/main.cf (remove relayhost).

```
smtp_sender_dependent_authentication = yes
sender_dependent_relayhost_maps = hash:/etc/postfix/sender_relay
smtp_sasl_auth_enable = yes
smtp_sasl_security_options = noanonymous
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
``` 

<div></div>
In /etc/postfix/sender_relay, list addresses that must go through a relay.

``` 
@example.com in.mailjet.com
example@example.net in.mailjet.com
```

<div></div> 
In /etc/postfix/sasl_passwd, provide credentials for each address listed in /etc/postfix/sender_relay.

``` 
@example.com apikey:secretkey
example@example.net apikey2:secretkey2
``` 

<div></div>
Don't forget the following commands.

``` 
cd /etc/postfix
chmod 600 sasl_passwd
chown root:root sasl_passwd
postmap sasl_passwd sender_relay
postfix reload
``` 

##Other configurations

- Desktop email clients (<a href="https://uk.mailjet.com/support/how-do-i-change-my-smtp-settings-in-outlook,98.htm" target="_blank">Outlook</a>, <a href="https://uk.mailjet.com/docs/apple-mail-smtp-setup" target="_blank">Apple Mail</a>, <a href="https://uk.mailjet.com/docs/thunderbird-smtp-setup" target="_blank">Thunderbird</a>)
- MTA & MDA (<a href="https://uk.mailjet.com/docs/code/exim" target="_blank">exim</a>)
- Frameworks and Languages (<a href="https://uk.mailjet.com/docs/code/php" target="_blank">Php</a>, <a href="https://uk.mailjet.com/docs/code/java" target="_blank">Java</a>, <a href="https://uk.mailjet.com/docs/code/asp" target="_blank">Asp.net</a>, <a href="https://uk.mailjet.com/docs/code/php/zend" target="_blank">Zend</a>, <a href="https://uk.mailjet.com/docs/code/php/codeigniter" target="_blank">CodeIgniter</a>)
- Webmail (<a href="https://uk.mailjet.com/support/how-to-set-up-mailjet-s-smtp-with-gmail,110.htm" target="_blank">Gmail and Google Apps</a>)


##Custom Headers

When using SMTP relay, you can use the following custom headers to specify how Mailjet will handle your message

###X-Mailjet-Campaign

This header value must be unique for all emails belonging to a specific campaign. This will regroup emails into only one line in your dashboard, and provide cool reports !
It can be an alpha-numeric value of your choice (space, dash and underscore are also accepted), up to 255 characters long on one line.

###X-Mailjet-DeduplicateCampaign

In combination with X-Mailjet-Campaign, this boolean (true, yes, y, 1) indicates that you do not wish to send a message inside the campaign twice to the same recipient. In this case, we check that the recipient hasn’t been sent the message, or otherwise block any duplicate (only the first message goes through).
Please note that this is based on recipient email address, it will not block a message sent to the same person on two different email addresses.

###X-MJ-CustomID

This custom value will help you track your message more easily.

###X-MJ-EventPayload

If you need more than an ID, no problem: insert a payload to your messages, using any format (XML, JSON, CSV…)

###X-Mailjet-TrackOpen

This header indicates that you want to activate or not the open tracking on the concerned message. This option will override your tracking options set on your user account.
 
 - <code>0</code>: disable the tracking <br />
 - <code>1</code>: enable the tracking


###X-Mailjet-TrackClick

This header indicates that you want to activate or not the click tracking on the concerned message. This option will override your tracking options set on your user account.

 - <code>0</code>: disable the tracking <br />
 - <code>1</code>: enable the tracking


###X-Mailjet-Prio

The header manage the different types of email by defining up to four priority levels.
<a href="https://app.mailjet.com/docs/email-priority-management" target="_blank">More information</a>

###X-MJ-TemplateLanguage

This hearder is related to the processing of the template language. It activate the template language processing. By default the template language processing is deactivated. Use <code>True</code> to activate.

[More information on transactional templating](#transactional-templating)

###X-MJ-TemplateID

This header allows to pass the ID of the template stored on the Mailjet system.

[More information on transactional templating](#transactional-templating)

###X-MJ-Vars

This hearder allows to pass a JSON encoded array of variables that can be used with the templating language.

Example: {"varname1": "value1","varname2": "value2", "varname3": "value3"}

###X-MJ-TemplateErrorReporting

This hearder is related to the processing of the template language. This header defines the email address where a carbon copy with error message is sent to.

[More information on transactional templating](#transactional-templating)

###X-MJ-TemplateErrorDeliver

This hearder is related to the processing of the template language. This header defines if the message is delivered when an error is discovered in the templating language. By default the delivery is deactivated. Use <code>deliver</code> to let the message be delivered to the recipient, <code>0</code> to stop it. 

[More information on transactional templating](#transactional-templating)

