+++
title = "Create a Secret"
chapter = true
weight = 10
+++

Open the [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/home#/home) and click on **Store a new secret**.

{{% img "Secrets Mgr.png" "Secrets Manager" %}} 

On the Store a new secret dialog make the following choices:

- Leave **Credentials for RDS database** selected
- Leave the default username as **admin**
- Specify the new password established in the prior lab **NewPassword123**; or the password you chose.
- Use the DefaultEncryptionKey
- Select the **rds-mysql-lab** RDS MySQL database you created during setup
- Click next

{{% img "Create-Secret.png" "Secrets Manager" %}} 

- Name your secret and give it a description.  **MySQLAdminPWD**
- Click Next

{{% img "Create-Secret2.png" "Secrets Manager" %}} 

- Leave the default, disabling rotation, on the next dialog

{{% img "Create-Secret3.png" "Secrets Manager" %}} 

- Review the different code samples illustrating ways to retreive your secret.  
- Click Store to save your new secret

{{% img "Create-Secret4.png" "Secrets Manager" %}} 

{{% img "Create-Secret5.png" "Secrets Manager" %}} 

Using your [AWS Cloud9](https://console.aws.amazon.com/cloud9/home) environment, validate you stored secret with the below command:

```
aws secretsmanager get-secret-value --secret-id MySQLAdminPWD
```

{{% notice tip %}}
See the contents of SecretString, this contains a key value pair string for the full details needed to connect to your RDS database as the masteruser, including the username, password, host, port, and database.
{{% /notice %}} 

``` json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:123481740215:secret:MySQLAdminPWD-tmfbng",
    "Name": "MySQLAdminPWD",
    "VersionId": "44498380-ff47-4b97-bade-18e226958260",
    "SecretString": "{\"username\":\"admin\",\"password\":\"NewPassword123\",\"engine\":\"mysql\",\"host\":\"rds-mysql-lab.cnsxdqacwvrb.us-east-1.rds.amazonaws.com\",\"port\":3306,\"dbname\":\"mylab\",\"dbInstanceIdentifier\":\"rds-mysql-lab\"}",
    "VersionStages": [
        "AWSCURRENT"
    ],
    "CreatedDate": 1605546490.689
}
```
