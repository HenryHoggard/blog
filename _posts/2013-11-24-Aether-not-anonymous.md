---
layout: post
title: Is Aether Really Anonymous?
tag: 
---


["tl;dr Aether is a distributed network that creates forum–like, anonymous and encrypted public spaces for its constituents. No registration necessary"](http://www.getaether.net)

I was really interested to hear about Aether, a new distributed forum-like software. Aether is marketed as an anonymous, distributed and encrypted forum system. This blog post is just a reminder to all, that like a lot of P2P software, it is not completely anonymous.

# Is Aether Anonymous?
From the point of view of accounts, yes, there is no extra information stored and tied to accounts on Aether, the only identifying information is the handle you choose, which is not tied to a single user, anyone can use that handle.

However from the point of view of IP addresses and connection information, no, using tools like netstat, wireshark and lsof, you can view other nodes connected to the network. So be aware that when you are posting your worldly views, that your IP address is easily accessible. So it is recommended to connect through a VPN or TOR.

```bash
~ sudo lsof -i -P

aether 23356 TCP 10.113.3.7:49819->82-60-93-222.dsl.in-addr.zen.co.uk:63677 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49820->178-190-233-146.adsl.highway.telekom.at:54590 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49821->host86-120-245-28.range86-129.btcentralplus.com:58426 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49822->chellj080108139058.2.12.vie.surfer.at:56355 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49824->62.80.69.138.dyn.user.ono.com:60594 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49825->cpc4-lee212-2-0-cust492.7-1.cable.virginm.net:64890 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49826->cable-213-30-236-69.zeelandnet.nl:2669 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49827->78.178.191.40.dynamic.ttnet.com.tr:53450 (SYN_SENT)
aether 23356 TCP 10.113.3.7:49828->c-174-61-6-50.hsd1.fl.comcast.net:59805 (ESTABLISHED)
aether 23356 TCP 10.113.3.7:49829->ip-31-205-61-124.ask4internet.com:61056 (SYN_SENT)
aether 23356 TCP 10.113.3.7:49830->0x3ec63d07.inet.dsl.telianet.dk:52074 (SYN_SENT)
aether 23356 TCP 10.113.3.7:49831->cpc8-cwma7-2-5-cust91.7-3.cable.virginm.net:34198 (SYN_SENT)
```
* certain information such as IP addresses, have been modified or stripped out for privacy reasons

But don't let that stop you, personally I love Aether, its aesthetically pleasing, easy to use, and a brilliant idea!