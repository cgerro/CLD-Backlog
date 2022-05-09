---
description: This page describes the notion of one for the Challenge 00
---

# C0 - Notion of done

### To validate this challenge your delivery must meet those requirements:

#### General requirements

* Provide a technical documentation:
  * Written in markdown.
  * Delivered in pdf.

{% file src="../../.gitbook/assets/SampleTechnicalDoc.zip" %}
Sample technical doc
{% endfile %}

#### Specific requirements

{% tabs %}
{% tab title="Part I - Console" %}
* technical documentation containing all the "how-to".
* A video is also accepted.
{% endtab %}

{% tab title="Part II - Launch instance" %}
An instance who meet the specification:

* subnet
* naming convention
* security group rules
* ....
{% endtab %}

{% tab title="Part III - SSH Access" %}
Technical documentation containing all commands used to:

* reach the DMZ
* reach the private instance (in the private subnet)

Your private instance:

* must host a file saved under your home's user folder.
* this is the file to upload on your instance

{% file src="../../.gitbook/assets/awsConsole.PNG" %}
File to host on the private instance
{% endfile %}

&#x20;
{% endtab %}

{% tab title="Part IV - AWS Cli" %}
Technical documentation containing the following commands and the result obtained:

* list all instances in the Milano region
* start your own instance
* stop your own instance
* terminate your own instance
* create an image (full backup) of your own instance
{% endtab %}
{% endtabs %}

