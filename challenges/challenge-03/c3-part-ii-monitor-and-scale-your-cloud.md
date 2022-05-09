---
description: This page describes the Part II of the Challenge 2
---

# C3 - Part II - Monitor and scale your cloud

### Use Case

Case 1 : As soon as the CPU load of the main Drupal instance reach 70%, the autoscaling mechanism must launch new instances (horizontal scaling) to absorb this load (max 4 in total).

Case 2 : As soon as the load goes down, we must observe the instances reduce in number until the return to normal operation status. &#x20;

### Step 0 - Read the doc

* [AWS - What is EC2 autoscaling ?](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

### Step 1 - Create a launch template

* [AWS - Tutorial](https://docs.aws.amazon.com/autoscaling/ec2/userguide/GettingStartedTutorial.html#gs-create-lt)
* Naming convention:  LCONFIG\_CLGDRP\[XX]
* Tips:
  * Use the same SG, same KeyPair as for your original instance.
  * To not use "Spot" Instance.

### Step 2 - Create a single-instance Auto Scaling group

* [AWS - Tutorial](https://docs.aws.amazon.com/autoscaling/ec2/userguide/GettingStartedTutorial.html#gs-create-asg)
* Naming convention : ASG\__CLDGRP\_\[XX]\__LABO3
*   Tips:

    * Step 3
      * Load balancing : Select "No load balancer"



### Step 3 - Experiment with your config

* Using Stress as in Part I, try to activate your autoscaling group and observe the instances automatically launched.
* After Stress sequence, observe how your infra return to "normal" operation.

&#x20;&#x20;





