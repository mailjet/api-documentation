# Concatenation and Encoding

When you send SMS with Mailjet, you should be aware of how many parts your message is being sent as, and what type of encoding is required to send your message.

## Overview

Every SMS message has a limit for the characters it may contain for the chosen encoding. If you send a message that contains more than that limit, Mailjet will send a concatenated SMS.

A concatenated SMS contains multiple (**up to 5**) SMS parts that are connected by segmentation information in the User Data Header (UDH).

Segmentation information tells the handset the number of messages that make up the concatenated SMS, and the position of each SMS part in the concatenated SMS. The parts of a concatenated SMS arrive at the user's handset out of sequence. When the handset has received all SMS parts, it presents your message as a single text to your user.

The maximum number of characters you can fit into an SMS part depends on the Encoding you are using.

Number of SMS messages | GSM7 | UNICODE
  ----------|------------|------------
1 standard SMS message | up to 160 characters | up to 70 characters
2 concatenated SMS messages | up to 306 characters | up to 134 characters
3 concatenated SMS messages | up to 459 characters | up to 201 characters
4 concatenated SMS messages | up to 612 characters | up to 268 characters
5 concatenated SMS messages | up to 765 characters | up to 335 characters

## GSM7 Encoding

> Example for message encoded in GSM7:

```json
{
  "From":"WineShop",
  "To":"+33600000000",
  "Text":"Wine has been produced for thousands of years, with the earliest wines being drunk c. 6000 BC in Georgia. It had reached the Balkans by c. 4500 BC and was consumed and celebrated in ancient Greece and Rome."
}
```

GSM-7 is a character encoding standard which packs the most commonly used letters and symbols in many languages into 7 bits each for usage on GSM networks. As SMS messages are transmitted 140 8-bit octets at a time, GSM-7 encoded SMS messages can carry up to 160 characters.

GSM-7 is the standard alphabet for SMS messages, written up in the standard [GSM 03.38](http://www.etsi.org/deliver/etsi_gts/03/0338/05.00.00_60/gsmts_0338v050000p.pdf). It is always supported on GSM networks.

The basic character set for GSM-7 can be found [here](https://en.wikipedia.org/wiki/GSM_03.38#GSM_7-bit_default_alphabet_and_extension_table_of_3GPP_TS_23.038_.2F_GSM_03.38).

The `Text` property in the SMS sample message contains 206 characters, which means that it will be sent as 2 concatenated parts.

<div></div>

## UNICODE (UTF-16) Encoding

>Example for message encoded in UNICODE:

```json
{
  "From":"WineShop",
  "To":"+33600000000",
  "Text":"Hello world, Καλημέρα κόσμε, コンニチハ. Wine has been produced for thousands of years, with the earliest wines being drunk c. 6000 BC in Georgia. It had reached the Balkans by c. 4500 BC and was consumed and celebrated in ancient Greece and Rome."
}
```

UNICODE is a character encoding standard in which characters are represented by a fixed-length 16 bits (2 bytes). It is used as a fallback on many GSM networks when a message cannot be encoded using GSM-7, or when a language requires more than 128 characters to be rendered.

UTF-16 is defined by the international Organization for Standardization (ISO) in [ISO 10646](http://standards.iso.org/ittf/PubliclyAvailableStandards/index.html).

UTF-16 represents a possible maximum of 65,536 characters, or in hexadecimals from 0000h - FFFFh (2 bytes). The characters in UTF-16 are synchronized to the [Basic Multilingual Plane](http://unicode.org/roadmaps/bmp/) in UNICODE.

<aside class="notice">
Note: Even if you use only a single character that is not supported by GSM7, the whole SMS will be encoded in UNICODE.
</aside>

The sample message contains Greek & Japanese characters, so it needs to be encoded in UNICODE. It contains 242 characters, hence it will be sent as 4 concatenated SMS parts.
