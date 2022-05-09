---
description: This page describes the SSH Challenge
---

# C0 - Part III - SSH access

### Prerequisites

* [Part II - Launch instance](c1-part-ii-launch-instance.md)
* A SSH Client (if you want to use Putty, you need to convert .pem to .ppk file)
* Read carefully the following Infra Schema

![Labo infra based on AWS Standard (see Challenge 00)](../../.gitbook/assets/CLD\_Infra\_Labo0.png)

### SSH Challenge - Step by Step

#### Step 01 - Reach the DMZ

```
//pattern
ssh cldgrp[XX]@[SSH Srv Public Ip] - i [PathTotheKey]/CLDGRP[XX].pem

//sample for GRP99
ssh cldgrp99@15.161.201.245 -i CLDGRP99.pem
Last login: Fri Feb 25 10:21:30 2022 from ******

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
```

#### Step 02 - Build SSH Tunnel to reach to private subnet

Build a SSH Tunnel from your localhost to the DMZ allowing you ssh connectivity with your private instance.

```
//pattern
//you need to add port forwading argument like this to the previous command:
-L [localPortToForward]:[IpOfYourPrivateInstance]:22

//sample for GRP99
ssh cldgrp99@15.161.201.245 -L 23:10.0.99.10:22 -i CLDGRP99.pem
Last login: Fri Feb 25 10:21:30 2022 from mob-194-230-158-184.cgn.sunrise.net

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
```

#### Step 03 - Open the session to your private instance

Then you can open a second connection to your private instance like this:

```
//pattern
ssh [sshUser]@localhost -i [PathTotheKey]/CLDGRP[XX].pem -p [localPortToForward]

//sample for GRP99
ssh ubuntu@localhost -p 23 -i KEY_CLDGRP99_UBU20_DRUPAL.pem
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.11.0-1028-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Feb 25 10:38:44 UTC 2022

  System load:  0.07              Processes:             112
  Usage of /:   27.2% of 7.69GB   Users logged in:       0
  Memory usage: 19%               IPv4 address for ens5: 10.0.99.10
  Swap usage:   0%


0 updates can be applied immediately.
```

### Final Step

Try to establish a ssh connexion with your Ubuntu instance and check if the web is reachable.

```
ubuntu@ip-10-0-[XX]-10:~$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=38 time=7.44 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=38 time=7.48 ms
```



