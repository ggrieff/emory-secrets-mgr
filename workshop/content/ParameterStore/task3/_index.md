+++
title = "Update RDS Credentials"
chapter = true
weight = 30
+++

Now you will establish a new parameter with a new MasterUser password, then you will update your RDS instance to use this new password.  However, this time you will use the [AWS CLI]() to create the parameter, then update the database with the value.

Create a new parameter with a secure string.  *Feel free to substitute a password of your choosing.*"
```
aws ssm put-parameter --name NewMasterUserPWD --type=SecureString --value "NewPassword123"
```

Validate that you can retieve the password.
```
aws ssm get-parameter --name NewMasterUserPWD
```

Now, update your RDS instance to this new password
```
aws rds modify-db-instance --db-instance-identifier \
   `aws rds describe-db-instances \
     --region $AWSREGION \
     --query 'DBInstances[*].DBInstanceIdentifier' \
     |jq -r '.[0]'` \
    --master-user-password \
      `aws ssm get-parameter --name NewMasterUserPWD \
       --with-decryption --query Parameter.Value  \
       | sed -e 's/^"//' -e 's/"$//'`
```
Breaking this command down:

- Using the AWS CLI for RDS
- Find the instance identifier of the the only RDS instance in the account.
- Decrypt the value for secure secret, NewMasterUserPWD, stored in Parameter store
- Modify this RDS instance, setting a new masteruser password to the decrypted value

With the command running Go to the [RDS Console](https://console.aws.amazon.com/rds/home#database:id=rds-mysql-lab) and ensure the MasterUser credentials have been updated and the status returns to **Available**.

{{% img "RDS resetting.png" "RDS Console" %}}

{{% img "RDS Available.png" "RDS Console" %}}

Once again validate connectivity.
```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws ssm get-parameter \
    --name NewMasterUserPWD --with-decryption \
    --query Parameter.Value \
    |sed -e 's/^"//' -e 's/"$//'` \
  mylab
SELECT * FROM USERINFO;
```

Confirm that you are able to see the contents of the USERINFO table created during setup.

You can move on to [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)!  by clicking the right arrow.