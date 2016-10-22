---
layout: post
title: Paypal 2FA Bypass
tag: 
---

_Recently I was in a hotel needing to make a payment, there was no phone signal so I could not receive my Two Factor Auth token. Luckily for me Paypal's 2FA took less than five minutes to bypass._


# Proof of Concept

**Step 1:** Login with a valid username and password, click on the "Try another way" link.

![loginform](/assets/images/verifynumber.png)

**Step 2:** Enter any answer for security questions.

![securityquestions](/assets/images/securityquestions.png)

**Step 3:** Using a proxy, remove "securityQuestion0" and "securityQuestion1" from the post data.

![postdata](/assets/images/postdata.png)

**Step 4:** Profit

![verified](/assets/images/verified.png)



# Advisory Timeline

* 03/10/16  - Reported issue to Paypal 
* 04/10/16  - Paypal begin investigation of issue
* 21/10/16 - Paypal report issue as fixed 
* 21/10/16 - Paypal award bounty 
