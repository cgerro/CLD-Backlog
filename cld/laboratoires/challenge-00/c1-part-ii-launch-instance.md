---
description: This page help you to launch a first instance (in a specific subnet)
---

# C0 - Part II - Launch Instance

### Prerequisites

* Get the credentials (see your dev ops teams channel in teams).
* Select the eu-south-1 region.
* Read carefully the following Infra Schema

![Labo infra based on AWS Standard (see Challenge 00)](../../../.gitbook/assets/CLD\_Infra\_Labo0.png)



### **Launch instance (step by step)**

#### Step 1: Choose an Amazon Machine Image

* AMI - Ubuntu 20.04 LTS (64-bit x86)&#x20;
  * ARM vs x86&#x20;

#### Step 2: Choose an Instance Type&#x20;

* t3.micro

#### Step 3: Configure Instance Details

* VPC : VPC-CLD
* Subnet : Subnet-private-CLDGRPXX
* Private-ip : 10.0.XX.10 (at the page's bottom - > network interface)

#### Step 4: Add Storage

* Stroage : 8 Go SSD&#x20;

#### Step 5: Add Tags

* Tag.Key = Name&#x20;
* Tag.Value = CLGGRPXX\_UBU20\_DRUPAL&#x20;

#### Step 6: Configure Security Group&#x20;

* Create a new security group : SG\_CLDGRPXX\_DRUPAL&#x20;
* Study the infra diagram (identify ports and protocols)[Stateless-full](https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-rules-engines.html)&#x20;

#### Step 7: Review Instance Launch&#x20;

* no action
* LAUNCH
* Create a new key pair
  * Key pair type [RSA or Ed25519](https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54)
  * Key\_CLDGRPXX\_UBU20\_DRUPAL
