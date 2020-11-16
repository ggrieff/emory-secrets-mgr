+++
title = "Manual Secret Rotation"
chapter = true
weight = 30
+++

In the same fashion you did in the Parameter Store lab, you can manually rotate your secret.  Use the below commands to establish a new secret with an updated password.

Let's start by using the Secrets Manager CLI command to generate a random secure password.  Using your **AWS Cloud9** terminal issue the following command: 

```
aws secretsmanager get-random-password | jq -r .RandomPassword
```

Run this command to generate a credentials file from your current secret.
``` ssh
aws secretsmanager get-secret-value --secret-id MySQLAdminPWD --query SecretString >> mycreds.json
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

And now, update your RDS MySQL database to use this value for its MasterUser's password.

Validate connectivity.

```
mysql -h $MYSQL_HOST -u $DBUSER -p`aws ssm get-parameter \
    --name NewMasterUserPWD --with-decryption \
    --query Parameter.Value \
    |sed -e 's/^"//' -e 's/"$//'` \
  mylab
SELECT * FROM USERINFO;
```

{{% img "Secrets Mgr cover image.png" "Secrets Manager" %}} 