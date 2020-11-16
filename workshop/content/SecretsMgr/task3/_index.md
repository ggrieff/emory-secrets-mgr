+++
title = "Manual Secret Rotation"
chapter = true
weight = 30
+++

In the same fashion you did in the Parameter Store lab, you can manually rotate your secret.  Use the below commands to establish a new secret with an updated password.

Let's start by using the Secrets Manager CLI command to generate a random secure password.  Using your **AWS Cloud9** terminal issue the following command: 

```
aws secretsmanager get-random-password --exclude-punctuation | jq -r .RandomPassword
```

Run this command to generate a credentials file from your current secret.

``` 
aws secretsmanager get-secret-value --secret-id MySQLAdminPWD --query SecretString --output text > mycreds.json
```

Using the Cloud9 IDE update the mycreds.json file and replace the password with your randomly generated string.

{{% img "created creds file.png" "Credentials" %}} 

{{% img "update pwd.png" "Credentials" %}} 

{{% notice warning %}}
Be sure to save the mycreds.json file once you have completed editing.  Ctrl-S will save.
{{% /notice %}} 

Store a new version of the secret using the below command
```
aws secretsmanager put-secret-value --secret-id MySQLAdminPWD --secret-string file://mycreds.json
```

``` json
{
    "ARN": "arn:aws:secretsmanager:us-east-1:123481740215:secret:MySQLAdminPWD-tmfbng",
    "Name": "MySQLAdminPWD",
    "VersionId": "33e0c54a-d4cc-4872-a991-f124186bb089",
    "VersionStages": [
        "AWSCURRENT"
    ]
}
```

Go to the [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/home) and validate your update by retrieving your secret and looking at the plain text value.

{{% img "Secret.png" "Credentials" %}} 

{{% img "retrieve value.png" "Credentials" %}} 

{{% img "plaintext.png" "Credentials" %}} 

And now, update your RDS MySQL database to use this value for its MasterUser's password.

```
aws rds modify-db-instance --db-instance-identifier \
   `aws rds describe-db-instances \
     --region $AWSREGION \
     --query 'DBInstances[*].DBInstanceIdentifier' \
     |jq -r '.[0]'` \
    --master-user-password \
      `aws secretsmanager get-secret-value \
    --secret-id MySQLAdminPWD --query SecretString --output text \
    | jq -r .password`
```

Confirm the RDS database credentials are updating and wait for the database to reach **Available** state.

{{% img "resetting creds.png" "Credentials" %}} 

Validate connectivity using the below command in your Cloud9 Environment.

```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws secretsmanager get-secret-value \
    --secret-id MySQLAdminPWD --query SecretString --output text \
    | jq -r .password` \
  mylab
SELECT * FROM USERINFO;
```

{{% img "final.png" "Credentials" %}} 
