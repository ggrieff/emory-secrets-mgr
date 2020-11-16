+++
title = "Connect with Parameter"
chapter = true
weight = 20
+++

In your Cloud9 terminal, use the below command to retrieve the password, and connect to MySQL

```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws ssm get-parameter \
    --name MasterUserAdminPWD \
    --with-decryption --query Parameter.Value \
    |sed -e 's/^"//' -e 's/"$//'` \
  mylab
```

Breaking this command down:

- Using the enviorment variables for your RDS EndPoint and MasterUserName gathered earlier. 
- Embedding aws ssm -get-parameter retrieves the parameter value you just created for the -p (password) switch.
- The --with-decription flag decrypts the secure string value
- The --query Parameter.Value only returns that element from the JSON response
- Finally we use sed to strip off the leading and trailing **"**

{{% img "Cloud9.png" "Console" %}} 

Validate the connection by querying you sample table.
``` sql
SELECT * FROM USERINFO;
```
{{% notice info %}}
To exit mysql type **\q**
{{% /notice %}}  

