+++
title = "Create a Secret"
chapter = true
weight = 10
+++

Open the [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/home#/home) and click on **Store a new secret**.

{{% img "Secrets Mgr.png" "Secrets Manager" %}} 

On the Store a new secret dialog make the following choices:

1. Leave **Credentials for RDS database** selected
2. Leave the default username as **admin**
3. Specify the new password established in the prior lab **NewPassword123**; or the password you chose.
4. Use the DefaultEncryptionKey
5. Select the **rds-mysql-lab** RDS MySQL database you created during setup
6. Click next

{{% img "Create-Secret.png" "Secrets Manager" %}} 

7. Name your secret and give it a description.  **MySQLAdminPWD
8. Click Next

{{% img "Create-Secret2.png" "Secrets Manager" %}} 

9. Leave the default, disabling rotation, on the next dialog

{{% img "Create-Secret3.png" "Secrets Manager" %}} 

10. Review the different code samples illustrating ways to retreive your secret.  
11. Click Store to save your new secret

{{% img "Create-Secret4.png" "Secrets Manager" %}} 

{{% img "Create-Secret5.png" "Secrets Manager" %}} 

