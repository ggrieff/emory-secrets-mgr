+++
title = "Parameter Store"
chapter = true
weight = 20
+++

In this lab you will use [AWS Systems Manager - Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) to save and retrieve the masteruser's password for your MySQL database.

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, Amazon Machine Image (AMI) IDs, and license codes as parameter values. You can store values as plain text or encrypted data. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter.

{{% img "Systems_Manager.png" "Systems Manager" %}}

{{% children %}}