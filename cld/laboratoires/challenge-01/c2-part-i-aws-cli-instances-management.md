# \[REVIEW] C1 - Part I - Managing Instances

### Prerequisites

* Get the credentials (see your dev-ops teams channel in teams).
* [Download and install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### Challenge

#### Step 01a - Launch Instances

We try to launch a new instance in the correct private subnet.

Instance type : t3.micro

AMI : ami-0f8ce9c417115413d (Ubuntu 20)

Subnet : CLD-SUBNET-PRIVATE-DEVOPSTEAM\[XX]

[Source](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html)

```
[Request]
//sample
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids sg-903004f8 --subnet-id subnet-6e7f829e
//for CLDGRP99
aws ec2 run-instances --image-id ami-0f8ce9c417115413d --count 1 --instance-type t3.micro --key-name KEY_CLDGRP99_UBU20_DRUPAL --security-group-ids sg-0c276b94077a68744 --subnet-id subnet-08172ed4631814cf1 --region eu-south-1 --profile CLDGRP99

[Response]
{
    "Groups": [],
    "Instances": [
        {
        [..]
        }
    ],
    "OwnerId": "709024702237",
    "ReservationId": "r-00ab95b681ea9e866"
}       
```

#### Step 01b - Launch instance - with Private Ip and Tag Name

As Step 01a, we try to launch an instance. Add in the CLI command the argument allowing to set:

Private IP: 10.0.\[XX].10

Tag Name : Value = "CLDGRP99\__UBU20\__DRUPAL"

```
[Request]
//TODO

[Response]
//TODO
```

#### Step 02 - Start Instances

We try to start both DMZ Instances (NAT-SRV and SSH-SRV) as well as the private instance:

```
[Request]
//                                     NAT-SRV             SSH-SRV             CLDGRP99 Instance    
aws ec2 start-instances --instance-ids i-0edd7b00f313a3908 i-0cd826608eb84934d i-0f39d46d11473061c --region eu-south-1 --profile CLDGRP99

[Response]
{
    "StartingInstances": [
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-0cd826608eb84934d",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        },
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-0edd7b00f313a3908",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        },
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-034e97efd737085e9",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        }
    ]
}
```

We check if the targeted instances have been stopped:

```
[Request]
aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query "Reservations[*].Instances[*].{Instance:InstanceId,Name:Tags[?Key=='Name']|[0].Value}" --output table --region eu-south-1 --profile CLDGRP99

[Response]
----------------------------------------------------
|                DescribeInstances                 |
+----------------------+---------------------------+
|       Instance       |          Name             |
+----------------------+---------------------------+
|  i-0edd7b00f313a3908 |CLD-INSTANCE-PUBLIC-NAT    |
|  i-0cd826608eb84934d |CLD-INSTANCE-PUBLIC-SSHSrv |
|  i-0f39d46d11473061c |CLD-INSTANCE-DEVOPSTEAM[00]|
+----------------------+---------------------------+
```

#### Step 03 - Stop Instances

We try to stop both DMZ Instances (NAT-SRV and SSH-SRV) as well as the private instance:

```
[Request]
//TODO

[Response]
//TODO
```

#### Step 04 - Backup Instances

We try to back up our private instance:

{% hint style="info" %}
Backup Naming convention : CLDGRP\[XX]\_\[FreeDescription]
{% endhint %}

```
[Request]
//TODO

[Response]
//TODO
```

#### Step 05 - Terminate Instances

We try to terminate our private instance:

```
[Request]
//TODO
[Response]
//TODO
```

#### Step 06 - Restore Instances from Backup

We try to launch a new instance from our AMI:

```
[Request]
//TODO
[Response]
//TODO
```



