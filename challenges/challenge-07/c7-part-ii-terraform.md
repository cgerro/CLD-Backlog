# C7 - Part II - Terraform

## Pedagogical objectives

* Deploy a web site in an automated fashion
* Become familiar with tools for cloud deployment and configuration management
* Apply the principles of Infrastructure-as-Code and Desired State Configuration
* (Optional) Use a simple CI/CD pipeline to manage your cloud infrastructure

## Tasks

In this lab you will perform a number of tasks and document your progress in a lab report. Each task specifies one or more deliverables to be produced. Collect all the deliverables in your lab report. Give the lab report a structure that mimics the structure of this document.&#x20;

## Overview

In this lab you will deploy a web site running on a virtual machine in the cloud using tools that adhere to the principles of Infrastructure-as-Code and Desired State Configuration. The web server is NGINX and the cloud is Google Cloud.

```
Infrastructure-as-Code: instead of using a web console to create resources manually by clicking buttons you will describe the infrastructure you want to create in a file as code. You put this file in a Version Control System like git and you treat it as other code files.
Desired State Configuration: instead of saying "first create this, then modify that" imperatively, you will describe the desired state declaratively. A tool will determine the actual state, compare it to the desired state, and figure out by itself what needs to be done to transform the actual state into the desired state.
```

In the first part we _provision_ the cloud infrastructure, i.e., we create the cloud resources (virtual machine) needed for our application. The tool for cloud provisioning is Terraform.

In the second part, we configure the virtual machine by installing a web server and its configuration files. The tool for configuration management is Ansible.

In the third (optional) part, we use Terraform in a team. For that, we need to share the Terraform state among the team members. Our solution is to store the Terraform state in a Version Control System.&#x20;

### Task 1: Download config files

{% file src="../../.gitbook/assets/cld-2021-2022-labgce-student-files.zip" %}

### Task 2: Create a cloud infrastructure on Google Compute Engine with Terraform

In this task you will create a simple cloud infrastructure that consists of a single VM on Google Compute Engine. It will be managed by Terraform.

This task is highly inspired from the following guide: Get started with Terraform.

Create a new Google Cloud project. Save the project ID, it will be used later.

```
Name: [CLDGRPXX]labgce
```

As we want to create a VM, you need to enable the Compute Engine API:

```
Navigate to https://console.cloud.google.com/flows/enableapi?apiid=compute.googleapis.com
```

Terraform needs credentials to access the Google Cloud API. Generate and download the Service Account Key:

```
Navigate to IAM & Admin > Service Accounts.
Click on the default service account > Keys and ADD KEY > Create new key (JSON format).
On your local machine, create a directory for this lab. In it, create a subdirectory named credentials and save the key under the name labgce-service-account-key.json, it will be used later.
```

Generate a public/private SSH key pair that will be used to access the VM and store it in the credentials directory:

```
ssh-keygen -t ed25519 -f [CLDGRPXX]labgce-ssh-key -q -N "" -C ""
```

At the root of your lab directory, create a terraform directory and get the backend.tf, main.tf, outputs.tf and variables.tf files (available at the end of this page) and put them there. These files allow you to deploy a VM, except for a missing file, which you have to provide. Your task is to explore the provided files and using the Terrform documentation understand what these files do.

The missing file terraform.tfvars is supposed to contain values for variables used in the main.tf file. Your task is to find out what these values should be. You can freely choose the user account name and the instance name (only lowercase letters, digits and hyphen allowed).

You should have a file structure like this:

```
.
├── ansible
│   ├── ansible.cfg
│   ├── hosts
│   └── playbooks
│       ├── files
│       │   └── nginx.conf
│       ├── templates
│       │   └── index.html.j2
│       └── web.yml
├── credentials
│   ├── labgce-service-account-key.json
│   ├── labgce-ssh-key
│   └── labgce-ssh-key.pub
└── terraform
    ├── backend.tf
    ├── main.tf
    ├── outputs.tf
    ├── terraform.tfstate
    ├── terraform.tfstate.backup
    ├── terraform.tfvars
    └── variables.tf
```

There are two differences between Google Cloud and AWS that you should know about:

```
Concerning the default Linux system user account created on a VM: In AWS, newly created VMs have a user account that is always named the same for a given OS. For example, Ubuntu VMs have always have a user account named ubuntu, CentOS VMs always have a user account named ec2-user, and so on. In Google Cloud, the administrator can freely choose the name of the user account.

Concerning the public/private key pair used to secure access to the VM: In AWS you create the key pair in AWS and then download the private key. In Google Cloud you create the key pair on your local machine and upload the public key to Google Cloud.
```

The two preceding parameters are configured in Terraform in the metadata section of the google\_compute\_instance resource description. For example, a user account named fred with a public key file located at /path/to/file.pub is configured as

metadata = { ssh-keys = "fred:${file("/path/to/file.pub")}" }

This is already taken care of in the provided main.tf file.

You can now initiliaze the Terraform state:

cd terraform terraform init

What files were created in the terraform directory? Make sure to look also at hidden files and directories (ls -a). What are they used for?

Check that your Terraform configuration is valid:

terraform validate

Create an execution plan to preview the changes that will be made to your infrastructure and save it locally:

terraform plan -input=false -out=.terraform/plan.cache

If satisfied with your execution plan, apply it:

terraform apply -input=false .terraform/plan.cache

If no errors occur, you have successfully managed to create a VM on Google Cloud using Terraform. You should see the IP of the Google Compute instance in the console. Save the instance IP, it will be used later.

After launching make sure you can SSH into the VM using your private key and the Linux system user account name defined in the terraform.tfvars file.

Deliverables:

```
Explain the usage of each provided file and its contents by directly adding comments in the file as needed (we must ensure that you understood what you have done). In the file variables.tf fill the missing documentation parts and link to the online documentation. Copy the modified files to the report.

Explain what the files created by Terraform are used for.

Where is the Terraform state saved? Imagine you are working in a team and the other team members want to use Terraform, too, to manage the cloud infrastructure. Do you see any problems with this? Explain.

What happens if you reapply the configuration (1) without changing main.tf (2) with a change in main.tf? Do you see any changes in Terraform's output? Why? Can you think of exemples where Terraform needs to delete parts of the infrastructure to be able to reconfigure it?

Explain what you would need to do to manage multiple instances.

Take a screenshot of the Google Cloud Console showing your Google Compute instance and put it in the report.
```

### Task 3: Install Ansible

Now that you have created a VM on Google Cloud, we can configure it to add all the required software for our needs. For that we are going to use Ansible. In this task you will install Ansible on your local machine.

The installation procedure depends on the OS on your local machine. If you run Windows it is recommended you use a Linux installation to run Ansible (Windows Subsystem for Linux with Debian/Ubuntu is fine).

To install on Linux: Use Python's package manager pip:

sudo pip install ansible

To install on macOS: Use the Homebrew package manager brew:

brew install ansible

Verify that Ansible is installed correctly by running:

ansible --version

You should see output similar to the following:

ansible \[core 2.12.5] config file = None configured module search path = \['/Users/ludelafo/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules'] ansible python module location = /usr/local/Cellar/ansible/5.7.1/libexec/lib/python3.10/site-packages/ansible ansible collection location = /Users/ludelafo/.ansible/collections:/usr/share/ansible/collections executable location = /usr/local/bin/ansible python version = 3.10.4 (main, Apr 26 2022, 19:43:24) \[Clang 13.0.0 (clang-1300.0.29.30)] jinja version = 3.1.2 libyaml = True

#### Task 4: Configure Ansible to connect to the managed VM

In this task you will tell Ansible about the machines it shall manage.

In the lab directory create a directory ansible. In this directory, create a file called hosts which will serve as the inventory file with the following content (in these instructions we have broken the file contents into multiple lines so that it fits on the page, but it should be all on one line in your file, without any backslashes):

gce\_instance ansible\_ssh\_host=\<managed VM's public IP address>\
ansible\_ssh\_user=\
ansible\_ssh\_private\_key\_file=

Replace the fields marked with < > with your values.

Verify that you can use Ansible to connect to the server:

ansible gce\_instance -i hosts -m ping

You should see output similar to the following:

gce\_instance | SUCCESS => { "ansible\_facts": { "discovered\_interpreter\_python": "/usr/bin/python3" }, "changed": false, "ping": "pong" }

We can now simplify the configuration of Ansible by using an ansible.cfg file which allows us to set some defaults.

In the ansible directory create the file ansible.cfg:

\[defaults] inventory = hosts remote\_user = private\_key\_file = host\_key\_checking = false deprecation\_warnings = false

Among the default options we also disable SSH's host key checking. This is convenient when we distroy and recreate the managed server (it will get a new host key every time). In production this may be a security risk.

We also disable warnings about deprecated features that Ansible emits.

With these default values the hosts inventory file now simplifies to:

gce\_instance ansible\_ssh\_host=\<managed VM's public IP address>

We can now run Ansible again and don't need to specify the inventory file any more:

ansible gce\_instance -m ping

The ansible command can be used to run arbitrary commands on the remote machines. Use the -m command option and add the command in the -a option. For example to execute the uptime command:

ansible gce\_instance -m command -a uptime

You should see output similar to this:

gce\_instance | CHANGED | rc=0 >> 18:56:58 up 25 min, 1 user, load average: 0.00, 0.01, 0.02

Deliverables:

```
What happens if the infrastructure is deleted and then recreated with Terraform? What needs to be updated to access the infrastructure again?
```

### Task 5: Install a web server and configure a web site

In this task you will configure the managed host to run an NGINX web server. This will necessitate four files:

```
The inventory file from the previous task (ansible/hosts).
A playbook with instructions what to configure (ansible/playbooks/web.yml).
The configuration file for NGINX (ansible/playbooks/files/nginx.conf).
A template for the home page of our web site (ansible/playbooks/templates/index.html.j2).
```

To make our playbook more generic, we will refer to our managed server not by its individual name, but we will create a group called webservers and put our server into it. We can then later easily add more servers which Ansible will configure identically.

Modify the file ansible/hosts by adding a definition of the group webservers, which for the time being contains exactly one server, gce\_instance:

\[webservers] gce\_instance ansible\_ssh\_host=52.23.169.86

You should now be able to ping the webservers group:

ansible webservers -m ping

The output should be the same as before.

Now get the web.yml playbook file (available at the end of this page) and put it in the ansible/playbooks directory

The playbook references the configuration file for NGINX. Get the nginx.conf file (available at the end of this page) and put it in the ansible/playbooks/files directory

The configuration file tells NGINX to serve the homepage from index.html. We'll use Ansible's template functionality so that Ansible will generate the file from a template.

Get the index.html.j2 template file (available at the end of this page) and put it in the ansible/playbooks/templates directory.

You should have a file structure like this

. ├── ansible │ ├── ansible.cfg │ ├── hosts │ └── playbooks │ ├── files │ │ └── nginx.conf │ ├── templates │ │ └── index.html.j2 │ └── web.yml ├── credentials │ ├── labgce-service-account-key.json │ ├── labgce-ssh-key │ └── labgce-ssh-key.pub └── terraform ├── backend.tf ├── main.tf ├── outputs.tf ├── terraform.tfstate ├── terraform.tfstate.backup ├── terraform.tfvars └── variables.tf

Now you can run the newly created playbook to configure NGINX on the managed host. The command to run playbooks is ansible-playbook and must be ran from the ansible directory:

ansible-playbook playbooks/web.yml

This should produce output similar to the following:

PLAY \[Configure webservers with nginx] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

TASK \[Gathering Facts] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* ok: \[gce\_instance]

TASK \[Install nginx] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* changed: \[gce\_instance]

TASK \[Copy nginx config file] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* changed: \[gce\_instance]

TASK \[Enable configuration] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* ok: \[gce\_instance]

TASK \[Copy index.html] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* changed: \[gce\_instance]

TASK \[Restart nginx] \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* changed: \[gce\_instance]

PLAY RECAP \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* gce\_instance : ok=6 changed=4 unreachable=0 failed=0

You can then test the new web site by pointing your browser to the address of the managed server. You should see the homepage showing "NGINX, configured by Ansible".

Deliverables:

```
Explain the usage of each file and its contents, add comments to the different blocks if needed (we must ensure that you understood what you have done). Link to the online documentation. Link to the online documentation.

Copy your hosts file into your report.
```

### Task 6: Adding a handler for NGINX restart

In this task you will improve the playbook by restarting NGINX only when needed.

The current version of the playbook restarts NGINX every time the playbook is run, just in case something in NGINX's configuration changed that the running NGINX process needs to pick up.

By putting the NGINX restart not into a task, but into a handler, its execution can be made conditional. What we want is that NGINX is only restarted if one of the tasks that affects NGINX's configuration had a change.

Consult the Ansible documentation about handlers. Modify the playbook so that the NGINX restart becomes a handler and the tasks that potentially modify its configuration use notify to call the handler when needed.

Deliverables:

```
Copy the modified playbook into your report.
```

### Task 7: Test Desired State Configuration principles

In this task you will do some tests to verify that Ansible implements the principles of Desired State Configuration.

According to this principle, before doing anything, Ansible should establish the current state of the managed server, compare it to the desired state expressed in the playbook, and then only perform the actions necessary to bring the current state to the desired state.

In its ouput Ansible marks tasks where it had to perform some action as changed whereas tasks where the actual state already corresponded to the desired state as ok.

```
Return to the output of running the web.yml playbook the first time. There is one additional task that was not in the playbook. Which one? Among the tasks that are in the playbook there is one task that Ansible marked as ok. Which one? Do you have a possible explanation?

Re-run the web.yml playbook a second time. In principle nothing should have changed. Compare Ansible's output with the first run. Which tasks are marked as changed?

SSH into the managed server. Modify the NGINX configuration file /etc/nginx/sites-available/default, for example by adding a line with a comment. Re-run the playbook. What does Ansible do to the file and what does it show in its output?

Do something more drastic like completely removing the homepage and repeat the previous question.
```

You can now destroy the infrastructure with Terraform

terraform destroy

Deliverables:

```
Answers to all the above questions.

What is the differences between Terraform and Ansible? Can they both achieve the same goal?

List the advantages and disadvantages of managing your infrastructure with Terraform/Ansible vs. manually managing your infrastructure. In which cases is one or the other solution more suitable?

Suppose you now have a web server in production that you have configured using Ansible. You are working in the IT department of a company and some of your system administrator colleagues who don't use Ansible have logged manually into some of the servers to fix certain things. You don't know what they did exactly. What do you need to do to bring all the server again to the initial state? We'll exclude drastic changes by your colleagues for this question.
```

### Task 8 (optional): Configure your infrastructure using a CI/CD pipeline

In this task we will simulate using Terraform in a team. The team members need to share Terraform, more precisely the Terraform state. For that we store the state in a Version Control System, GitLab.

GitLab gives us also the possibility to set up a CI/CD pipeline. We use the pipeline to manage the cloud infrastructure. This task is highly inspired from the following guide: Infrastructure as Code with Terraform and GitLab.

If not already done, create a GitLab account

https://gitlab.com/users/sign\_in

Add your public SSH key to GitLab (use the one creating for Google Cloud if you don't have any SSH key for your other projects)

https://gitlab.com/-/profile/keys

Create a new blank project, name it "labgce", make it public and uncheck the "Initialize repository with a README" checkbox. Leave other options as default

https://gitlab.com/projects/new

In your working directory, initialize the GitLab repository but do not commit yet

cd git init --initial-branch=main git remote add origin git@gitlab.com:/.git

In the ansible directory, create a .gitignore file containing the following lines

#### Ansible

.cfg files, which are likely to contain sentitive data

\*.cfg

In the credentials directory, create a .gitignore file containing the following lines

Ignore everything to avoid credential leaks

* Except this file !.gitignore

In the terraform directory, create a .gitignore file containing the following lines

```
## Terraform
## Local .terraform directory
.terraform
.tfstate files
*.tfstate .tfstate.
.tfvars files, which are likely to contain sentitive data
*.tfvars
```



Make a README.md file within your repository and add the following information:

* Laboratory title
* Group members
* Name of the group
* A quick description of the repository

You can now commit the current state of the repository

```
git add . git commit -m "Initial commit" git push -u origin main
```

We will now set up the GitLab CI configuration file. Get the .gitlab-ci.yml at the end of this page and store it at the root of the repository.

Edit the terraform/backend.tf file to this

terraform { backend "http" { } }

We must now add the environment variables so they are available to GitLab CI during the execution of the steps (leave all default options).

GCP\_PROJECT\_ID: The ID of the Google Cloud project GCP\_SERVICE\_ACCOUNT\_KEY: The content of the `labgce-service-account-key.json` file GCE\_INSTANCE\_NAME: The name of the instance GCE\_INSTANCE\_USER: The user of the instance SSH\_PUBLIC\_KEY: The content of the `labgce-ssh-key.pub` file SSH\_PRIVATE\_KEY: The content of the `labgce-ssh-key` file

You can now commit the new file. When pushed, a pipeline should be started (it can take a few minutes as we have a free plan) and will allow you to manage your infrastructure within GitLab CI.

Deliverables:

* Explain the usage of each file and its contents, add comments to the different blocks if needed (we must ensure that you understood what you have done). Link to the online documentation.
*   Explain what CI/CD stands for.

    Where is the Terraform state saved? Paste a screenshot where you can see the state with its details in GitLab.
*   Paste a link to your GitLab repository as well as a link to a successful end-to-end pipeline (creation, configuration and destruction of your infrastructure).

    Why are some steps manual? What do we try to prevent here?
* List the advantages and disadvantages of managing your infrastructure locally or on a platform such as GitLab. In which cases is one or the other solution more suitable?

