---
description: This page describes the SSH Challenge
---

# C0 - Part III - SSH access

### Prerequisites

* [Part II - Launch instance](../../../archives/session-2021-2022/challenges/challenge-00/c1-part-ii-launch-instance.md)
* A SSH Client (if you want to use Putty, you need to convert .pem to .ppk file)

### SSH Challenge - Step by Step

#### Step 01 - Reach the DMZ

```
//pattern
ssh [user]@[SSH Srv Public Ip] - i [PathTotheKey]/[KeyName].pem

//sample delivered in your private channel in teams
```

#### Step 02 - Build SSH Tunnel to reach to private subnet

Build a SSH Tunnel from your localhost to the DMZ allowing you ssh connectivity with your private instance.

```
//pattern
//you need to add port forwading argument like this to the previous command:
-L [localPortToForward]:[IpOfYourPrivateInstance]:22
```

#### Step 03 - Open the session to your private instance

Then you can open <mark style="color:red;">**a second session to connect**</mark> to your private instance like this:

```
//pattern
ssh [sshUser]@localhost -i [PathTotheKey]/CLDGRP[XX].pem -p [localPortToForward]
```

### Final Step

Try to establish a ssh connection with your Ubuntu instance and check if the web is reachable.

```
ubuntu@ip-10-0-[XX]-10:~$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=38 time=7.44 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=38 time=7.48 ms
```



