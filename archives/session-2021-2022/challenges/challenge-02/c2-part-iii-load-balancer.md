---
description: This page describes the Part III of the Challenge 2
---

# C2 - Part III - Load Balancer

## Create and deploy a Web Load Balancer

****[**AWS Official doc**](https://aws.amazon.com/elasticloadbalancing/)****

### **Step 1: Create a Target Group**

| Setting          | Value                                                                                         |
| ---------------- | --------------------------------------------------------------------------------------------- |
| Basic config     | <ul><li>Target type: instances</li><li>Target group name: LB-TB-CLDGRP[XX]</li></ul>          |
| Health checks    | <ul><li>HTTP</li><li>Port 8080</li><li>Health check path: "/drupal" </li></ul>                |
| Register targets | <ul><li>Choose your instance and include it as pending</li><li>Create target group </li></ul> |

### **Step 2: Create a Loadbalancer**

| Setting                         | Value                                                                                                                           |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Type of Loadbalancer            | Application Load Balancer                                                                                                       |
| Configure Load Balancer         | <ul><li>Name: LB-CLDGRP[XX]</li><li>Scheme : Internal</li><li>IP address type: ipv4</li></ul>                                   |
| Network mapping                 | <ul><li>VPC: VPC-CLD-LABO02</li><li><p>SUBNETS:</p><ul><li>10.0.[XX].0/28 -> A</li><li>10.0.[XX[.128/28 - B</li></ul></li></ul> |
| Configure Security Groups       | <p></p><ul><li>Port range: 8080</li><li>Protocol: TCP</li><li>Source: Reverse Proxy</li></ul>                                   |
| Configure Listeners and routing | <ul><li>HTTP:8080</li></ul>                                                                                                     |

* Put the Loadbalancer FQDN in Teams and tag Rémi Poulard
  * sample:&#x20;
* Wait for a response from Rémi
* Test your infra by using cldgrpXX.cld.education in a browser

### **Step 3: Add an instance**

* Using the custom virtual machine image you created earlier launch a second instance.
* Add this instance to the Loadbalancer target group

### Conceptual aspects

* Question 1 - Why does AWS need multiple AZ for Elasticity?
  * &#x20;[Official Doc](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
* Question 2 - How to prove the correct operation of the load balancer?



### Debug

* [How to debug Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html)
* [Update Time out config on Load Balancer?](https://aws.amazon.com/blogs/aws/elb-idle-timeout-control/)
* Check the security group... the Load Balancer is hosted in the same private subnets as your drupal instance...
* If your web app drupal does not run under "/drupal" but directly as the root path "/", please inform the assistant (impact on the reverse proxy setting)



