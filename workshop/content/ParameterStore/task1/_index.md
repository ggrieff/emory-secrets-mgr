+++
title = "Create Parameter"
chapter = true
weight = 10
+++

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

