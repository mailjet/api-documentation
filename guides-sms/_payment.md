# Payment and billing

## How to Fund Your SMS Wallet

In order to properly use the Send SMS API to send messages, you need to add money to your SMS wallet first.

To do that, please go to the [SMS Subscription page](https://app.mailjet.com/sms/subscription). You can deposit in EUR, GBP or USD, with a minimum deposit amount of 1 unit.

## SMS Wallet Currency

Please keep in mind that the deposit currency will determine your SMS wallet currency - e.g. if you make your deposit in EUR, the SMS wallet will also take EUR as its base currency.

### SMS Wallet Currency Change

The Mailjet account currency (and subsequently, the one for your SMS wallet) can be changed by depositing into your SMS wallet, or paying for your Mailjet subscription plan, in a different currency.

However, when the currency is changed, the funds already present in the wallet **will not be converted** into the new currency. Instead, the SMS service will be switched to a different wallet in the new currency. The amount in the original wallet will become inaccessible until such a time, when you switch to it with a new money transfer in the respective currency.

__Example:__

 - You make your first deposit of 100 EUR into your SMS wallet. The funds are added to your balance, and you can start using them right away. Any SMS you send will be charged based on the pricing in EUR.
 - You have 10 EUR left and decide to deposit more money. You transfer 100 USD to your wallet.
 - Your Mailjet account's currency is then changed to USD and you have 100 USD in the account, available for use. The 10 EUR you had left are not usable, until you change the currency back to EUR.
 - At some point in the future, you use up the 100 USD, and deposit 100 EUR once again. Now the currency is changed back to EUR, and you have 110 EUR available for use - 100 from the last deposit, and another 10 from the unused EUR balance you had left.

### Waiting Period after Initial Deposit

Upon your first deposit into your SMS wallet Mailjet needs to complete mandatory security checks, which take 48 hours, before you are able to send messages. Meanwhile the payment will be put on hold. Once the checks are complete and no issue with your account is detected, we will proceed with the payment and subsequent activation of your SMS wallet.

Keep in mind that the 48-hour verification period will be in effect every time you deposit with a new credit card.

## SMS Billing

Upon each SMS send request, your SMS wallet will be automatically billed. The cost depends on the [pricing for the respective country](https://www.mailjet.com/pricing/#sms), as well as the number of SMS parts the message consists of, indicated by the `SMSCount` property.

### Automatic Balance Check

Before the SMS message is sent, your SMS wallet balance will be automatically checked to ensure that there are enough funds to cover the cost of sending the message. If there are insufficient funds, an error message will be returned.

### Return Transactions

In cases where your SMS wallet is billed for sending the message, but the SMS could not be sent out (e.g. due to an issue with the processor), the transaction will be automatically reverted.

### Use of SMS API by Sub-accounts

Since token generation is only available on a master account, currently it is not possible for sub-accounts to use the SMS API.
