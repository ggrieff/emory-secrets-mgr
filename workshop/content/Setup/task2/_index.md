+++
title = "Setup Resources"
chapter = true
weight = 20
+++

Using [AWS Cloudformation](https://console.aws.amazon.com/cloudformation/home#/stacks) launch a stack to configure your lab enviorment using the button below.

Once your stack is created goto [AWS Cloud9](https://console.aws.amazon.com/cloud9/home) and Open the IDE.  Using the (+) open a terminal window.  Leave this tab open for the duration of the workshop we will be using it in future sections.

{{% img "labstack-Cloud9-IDE-AWS-Cloud9.png" "Console" %}}  

In the new terminal, paste and run these commands

```
# Install required tools for PostgreSQL
sudo yum install -y jq
sudo yum install -y postgresql96-contrib postgresql96 sysbench

```