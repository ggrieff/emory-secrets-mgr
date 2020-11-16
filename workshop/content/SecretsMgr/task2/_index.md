+++
title = "Use the Secret"
chapter = true
weight = 20
+++

Similar to using the secure string from **AWS Systems Manager - Parameter Store** we will now connect to the database using the secret using the AWS CLI.

Use the command below to connect to your MySQL RDS database.

```
mysql -h $MYSQL_HOST -u $DBUSER -p`\aws secretsmanager get-secret-value \
    --secret-id MySQLAdminPWD --query SecretString --output text \
    | jq -r .password` \
  mylab
```

Breaking this command down:
- Connect to the MySQL the previously established environment variables MYSQL_HOST and DBUSER.  
- Extract the password from our stored secret, identified by its name MySQLAdminPWD, querying for just the SecretString and parsing for the password with JQuery

Validate connection by using this SQL statement to return the results of your sample table

``` sql
SELECT * FROM USERINFO;
```

{{% notice info %}}
To exit mysql shell type **\q**
{{% /notice %}} 


{{% img "Cloud9-MySQL.png" "Cloud9 MySQL with Secrets Manager" %}} 