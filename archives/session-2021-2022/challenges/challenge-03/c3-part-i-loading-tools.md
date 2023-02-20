---
description: This page describes the Part I of the Challenge 3
---

# C3 - Part I - Loading tools

### Step 0 - Testing policy

* [AWS Penetration Testing](https://aws.amazon.com/security/penetration-testing/)
* [AWS DDoS Testing Policy ](https://aws.amazon.com/security/ddos-simulation-testing/)

### Step 1 - JMeter

1. Download and install on your local machine the JMeter tool from [http://jmeter.apache.org/](http://jmeter.apache.org/).
2. Open two terminal windows side-by-side and, using SSH, log into each instance. Bring up a continuous display of the Apache access log by running the command **sudo tail -F /var/log/apache2/access.log**.
3. Using the AWS console, enable detailed (1-minute interval) monitoring of the two instances: Select an instance and click on the **Monitoring** tab. Click on **Enable Detailed Monitoring**.
4. Consult the JMeter documentation [https://jmeter.apache.org/usermanual/build-web-test-plan.html](https://jmeter.apache.org/usermanual/build-web-test-plan.html) and create a simple test plan. Specify the load balancer as the target for the HTTP requests. Run a test.
5. Observe which of the instances gets the load. Increase the load and re-run the test. Observe response times and time-outs. Repeat until you see unacceptable response times and/or time-outs.
6. Immediately after having created a high load for the site, re-run the nslookup command to resolve the DNS name of the load balancer into IP addresses to see if there are any changes.

### Step 2 - Stress

1. Setup [htop](https://htop.dev/) and [stress](http://manpages.ubuntu.com/manpages/focal/man1/stress.1.html) on your drupal instance.
2. Stress your instance and observe it with htop.
3. Observe the monitoring view on the AWS Console.

### Step 3 - Analysis

* When you resolve the DNS name of the load balancer into IP addresses while the load balancer is under high load what do you see? Explain.
* Did this test really test the load balancing mechanism? What are the limitations of this simple test? What would be necessary to do realistic testing?

&#x20;
