---
description: This page describes the Part II of the Labo 2
---

# C1 - RDS

## Create and migrate a database using the Relational Database Service (RDS)

****[**AWS Official Doc**](https://aws.amazon.com/rds/)****

## Prerequisites

* [ ] Create an AMI of your Drupal Instance (CLD-INSTANCE-DEVTEAM\[XX])

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* [ ] Terminate your instance

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* [ ] Delete your private subnet

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* [ ] Create two new subnets sharing the ip range like this
  * [ ] 10.0.\[XX].0/28 -> eu-south-1a -> <mark style="color:red;">CLD-SUBNET-PRIVATE-DEVOPSTEAM\[XX]\_A</mark>
  * [ ] 10.0.\[XX].128/28 -> eu-south-1b -> <mark style="color:red;">CLD-SUBNET-PRIVATE-DEVOPSTEAM\[XX]\_B</mark>

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Result expected</p></figcaption></figure>

### **Step 1: Create DB subnet group**

{% hint style="info" %}
Go to AWS "RDS MANAGEMENT CONSOLE"
{% endhint %}

* Subnet group details

| Variable    | Value                               |
| ----------- | ----------------------------------- |
| Name        | CLD-DB-SUBNET-GROUP-DEVOPSTEAM\[XX] |
| Description | CLD-DB-SUBNET-GROUP-DEVOPSTEAM\[XX] |
| VPC         | CLD-VPC                             |

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* Add subnets

| Variable          | Value                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------- |
| Availabilily zone | eu-south-1a + eu-south-1b                                                               |
| Subnets           | <ul><li>10.0.[XX].0/28 -> eu-south-1a</li><li>10.0.[XX].128/28 -> eu-south-1b</li></ul> |

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>Result expected</p></figcaption></figure>

### **Step 2: Create a new security group for the RDS instance**

| Rule                 | Value                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------- |
| Name                 | <mark style="color:red;">CLD-SG-DEVOPSTEAM\[XX]\_RDS</mark>                                        |
| Rule for SQL Traffic | <ul><li>Port range: 3306</li><li>Protocol: TCP</li><li>Source: Private Subnet e-south-1a</li></ul> |

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

### **Step 3: Create a database**

{% hint style="info" %}
**This step will be performed by AWS Console.**\
****\
**Issue 1: no way  to specify the template with the CLI (prod/dev)**

**Issue 2: accessibility problem for the sql user created with the CLI.**
{% endhint %}

| Variable               | Value                                                                                                                                                                                                                  |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Engine                 | Please refer to your Drupal version                                                                                                                                                                                    |
| Template               | Dev / Test                                                                                                                                                                                                             |
| DB instance identifier | DBINS-DEVOPSTEAMS\[XX]-RDS-DRUPAL                                                                                                                                                                                      |
| DB User                | admin                                                                                                                                                                                                                  |
| DB Password            | \[pwd]                                                                                                                                                                                                                 |
| DB instance class      | <ul><li>Burstable classes</li><li>db.t3.micro</li></ul>                                                                                                                                                                |
| Storage                | <ul><li>Storage type: General Purpose (SSD)</li><li>Allocated Storage: 20 GB</li><li>Storage Autoscalling: unchecked</li></ul>                                                                                         |
| Connectivity           | <ul><li>VPC: <mark style="color:red;">VPC-CLD</mark></li><li>Subnet Group: The one created just before</li><li>Public access: No</li><li>VPC security group: use existing</li><li>Availability zone : A Zone</li></ul> |
| Additionnal option     | Disable autobackup                                                                                                                                                                                                     |

{% hint style="info" %}
Disable Monitoring option
{% endhint %}

{% hint style="info" %}
Disable all Additional configuration options
{% endhint %}

### **Step 4: Test connection**

* Get the RDS endpoint
* Test that the Drupal machine can connect to the RDS with the command:

```
mysql --host=endpoint_address --user=<rds_master_username> --password=<rds_master_password>`
If you have a prompt with `mysql>` it means that it worked
```

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

### **Step 5: Migrate DB**

* (In prod, you should inform end user for maintenance)
* Clean active sessions
* Connect to the database and create the database and a new user (rds\_admin pahX2dVhrLRncD)

```sql
CREATE DATABASE drupal;
CREATE USER 'rds_admin'@'%' IDENTIFIED BY '<rds_admin_password>';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON drupal.* TO 'rds_admin'@'%';
```

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* Disconnect from RDS and use this command to migrate data from local database to the rds

```shell
mysqldump --add-drop-table --no-tablespaces --user=drupal --password=<mysql_local_password> drupal | mysql --host=endpoint_address --user=<rds_admin_password> --password=<rds_admin_password> drupal
```

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

* Edit the database settings in file `/var/www/html/drupal/sites/default/settings.php`

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

### **Step 6 : Create a custom virtual machine image**

* In the EC2 console bring up the Instances panel and select the Drupal master instance.
* Bring up the context menu and select Image > Create Image. Provide the following answers (leave any field not mentioned at its default value):
* Image Name: DEVOPSTEAM\[XX]-RDS-DRUPAL-AMI.
* Image Description: Drupal connected to RDS database.
* Click Create Image. The instance will shut down temporarily and the image will be created.

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```

### Conceptual aspects

Question 1 - Standard vs Memory-optimized vs Burstable?

![](../../../.gitbook/assets/DBInstanceClass.PNG)

Question 2 - What's a Standby instance?

![](../../../.gitbook/assets/AvailabilityAndDurability.PNG)

Question 3 - How to prove the correct operation of the RDS?

```
[INPUT]
//TODO
[OUTPUT]
//TODO
```
