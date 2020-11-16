+++
title = "Parameter Store"
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

In your Cloud9 terminal, use the below command to retrieve the password, and connect to MySQL

```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws ssm get-parameter --name MasterUserAdminPWD --with-decryption --query Parameter.Value|sed -e 's/^"//' -e 's/"$//'` mylab
```

Breaking this command down:
- Using the enviorment variables for your RDS EndPoint and MasterUserName gathered earlier
- Embedding aws ssm -get-parameter retrieves the parameter value you just created for the -p (password) switch.
- The --with-decription flag decrypts the secure string value
- The --query Parameter.Value only returns that element from the JSON response
- Finally we use sed to strip off the leading and trailing **"**

Validate the connection by querying you sample table.
``` sql
SELECT * FROM USERINFO;
```
{{% notice info %}}
To exit mysql type **\q**
{{% /notice %}}  

Now we will establish a new parameter with a new MasterUser password, then we will update our RDS instance to use this new password.  However, this time we will use the [AWS CLI]() to create the parameter, then update the database with the value.

Create a new parameter with a secure string.  *Feel free to substitute a password of your choosing.*"
```
aws ssm put-parameter --name NewMasterUserPWD --type=SecureString --value "NewPassword123"
```

Validate that you can retieve the password.
```
aws ssm get-marater --name NewMasterUserPWD
```

Now, update your RDS instance to this new password
```
aws rds modify-db-instance --db-instance-identifier `aws rds describe-db-instances --region $AWSREGION --query 'DBInstances[*].DBInstanceIdentifier'|jq -r '.[0]'` --master-user-password `aws ssm get-parameter --name NewMasterUserPWD --with-decryption --query Parameter.Value | sed -e 's/^"//' -e 's/"$//'`
```
Breaking this command down:
- Using the AWS CLI for RDS
- Find the instance identifier of the the only RDS instance in the account.
- Decrypt the value for secure secret, NewMasterUserPWD, stored in Parameter store
- Modify this RDS instance, setting a new masteruser password to the decrypted value

With the command running Go to the [RDS Console](https://console.aws.amazon.com/rds/home#database:id=rds-mysql-lab) and ensure the MasterUser credentials have been updated and the status returns to **Available**.

Now, once again validate connectivity.
```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws ssm get-parameter --name NewMasterUserPWD --with-decryption --query Parameter.Value|sed -e 's/^"//' -e 's/"$//'` mylab
SELECT * FROM USERINFO;
```

Let's move on to [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)!  Click the right arrow.