+++
title = "Create Parameter"
chapter = true
weight = 10
+++

In this lab you will use AWS Systems Manager - Parameter Store to save and retrieve the masteruser's password for your MySQL database.

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, Amazon Machine Image (AMI) IDs, and license codes as parameter values. You can store values as plain text or encrypted data. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter. 

Open the [AWS Systems Manager - Parameter Store](https://console.aws.amazon.com/systems-manager/parameters)

{{% img "AWS-SYstems-Manager-Parameter-Store.png" "Console" %}} 

Create a parameter to store the MasterUser's password for your RDS MySQL database.
- Name: `MasterUserAdminPWD`
- Description: `MasterUser's (Admin) Password for RDS MySQL Databases`
- Tier: `Standard`
- Type `Secure String`
- KMS Key ID: `alias/aws/ssm/`
- Value `Test1234!`

{{% img "Create Parameter.png" "Console" %}} 

{{% img "Stored Parameter.png" "Console" %}} 

