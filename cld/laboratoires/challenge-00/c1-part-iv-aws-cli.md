---
description: This page describes the AWS CLI Challenge
---

# C0 - Part IV - AWS CLI

### Prerequisites

* Get the credentials (see your dev-ops teams channel in teams).
* [Download and install the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

#### Step 01 - Initial setup

Once the CLI is installed, we need to set up the profile we want to use with the CLI.

**Solution for a single profile**:

```
aws configure
AWS Access Key ID [********************]:
AWS Secret Access Key [********************]:
Default region name [None]:
Default output format [json]:
```

**Solution for a multiple profiles:**

Open the file "credentials" from there C:\Users\\\[yourUserName]\\.aws and add a new profile like this:

```
//add this kind of bloc at the end of credentials
[ProfileName] //do not need to be the same as the iam user
aws_access_key_id = ****
aws_secret_access_key = ****
```

Then, do not forget to add the argument --profile to your CLI command.

#### Step 02 - Get the list of instances

```
[Request]
//with default profile
aws ec2 describe-instances

//with specific profile
aws ec2 describe-instances --profile CLDGRP99 --region eu-south-1

[Response]
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-074b35d22662678b3",
                    [...]
```

#### Step 03 - Start A list of instances

```
[Request]
aws ec2 start-instances --instance-ids i-0edd7b00f313a3908 i-0cd826608eb84934d i-034e97efd737085e9 --region eu-south-1 --profile CLDGRP99
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
