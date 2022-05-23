# C7 - Part I - Prerequisites

## Setup Terraform Client

In this task you will install [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) on your local machine.

The installation procedure depends on the OS on your local machine. If you run Windows it is recommended you use a Linux installation to run Terraform (Windows Subsystem for Linux with Debian/Ubuntu is fine).

To install on Linux: Use Debian/Ubuntu's package manager apt:

### On Linux

#### Install the required packages to add Terraform APT repository

```
[Request]
  sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
```

```
[Request]
sudo apt-get update

[Response]
Get:1 http://security.debian.org/debian-security bullseye-security InRelease [44.1 kB]
Get:2 http://cdn-aws.deb.debian.org/debian bullseye 
[...]
Reading package lists... Done
```

```
[Request]
sudo apt-get install -y gnupg software-properties-common curl
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
curl is already the newest version (7.74.0-1.3+deb11u1).
The following additional packages will be installed:
  dirmngr gir1.2-glib-2.0 gir1.2-packagekitglib-1.0[ gnupg-l10n gnupg-utils gpg gpg-agent gpg-wks-client
  gpg-wks-server gpgconf gpgsm libappstream4 libassuan0 libdw1 libgirepository-1.0-1 libglib2.0-bin
  libglib2.0-data libgstreamer1.0-0 libksba8 libnpth0 
[...]

[Response]
Processing triggers for libc-bin (2.31-13+deb11u3) ...
Processing triggers for man-db (2.9.4-2) ...
Processing triggers for dbus (1.12.20-2) ...
```

#### Add the HashiCorp GPG key.

```
[Request]
  curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

[Response]
  Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead
```

#### Add the official HashiCorp Linux repository.

```
[Request]
  sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs ) main

[Response]
  -none-
```

#### Update to add the repository, and install the Terraform CLI.

```
[Request]
sudo apt-get update && sudo apt-get install terraform

[Response]
Hit:1 http://security.debian.org/debian-security bullseye-security InRelease
[...]
Setting up terraform (1.1.9) ...
```

### On macOS: Use the Homebrew package manager brew:

#### Install the HashiCorp tap
brew tap hashicorp/tap

#### Install Terraform
brew install hashicorp/tap/terraform

### Verify that Terraform is installed correctly by running:

```
[Request]
terraform --version

[Response]
Terraform v1.1.9
on linux_amd64

Your version of Terraform is out of date! The latest version
is 1.2.0. You can update by downloading from https://www.terraform.io/downloads.html
```

## Create A COULD INFRASTRUCTURE ON GOOGLE COMPUTE ENGINE WITH TERRAFORM

### Introduction

In this task you will create a simple cloud infrastructure that consists of a single VM on Google Compute Engine. It will be managed by Terraform.

This task is highly inspired from the following guide: [Get started with Terraform](https://cloud.google.com/docs/terraform/get-started-with-terraform).

Create a new Google Cloud project. Save the project ID, it will be used later.

### Create Google project

* [How to create a new google project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#gcloud)

```
[Request]
gcloud projects create cldlgrp99_labgce

[Response]
Create in progress for [https://cloudresourcemanager.googleapis.com/v1/projects/cldlgrp99_labgce].
Waiting for [operations/cp.7874600527401530437] to finish...done.
Enabling service [cloudapis.googleapis.com] on project [cldgrp99_labgce]...
Operation "operations/acat.p2-755723429290-6ab67b23-a4ec-4d42-b173-a4b15ea9ff03" finished successfully.
```

* [Enable billing](https://cloud.google.com/billing/docs/how-to/modify-project?hl=en_GB&_ga=2.198968175.-17430027.1634032196&_gac=1.119938554.1650551892.Cj0KCQjwgYSTBhDKARIsAB8KukutQk2FeBf9NuLa2s39o-ObVSYRbgVL3Fsj8NHmS6RPx-AilC1TrGAaAu-jEALw_wcB)


* [Enable API](https://console.cloud.google.com/flows/enableapi?apiid=compute.googleapis.com)