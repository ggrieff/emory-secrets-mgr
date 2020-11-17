+++
title = "IAM Considerations"
chapter = true
weight = 40
+++

Thus far we have used priviledged (admin) accounts to interact with AWS Secrets Manager, but how should policies be setup to restrict access to the masteruser credentials we have saved in [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)

Below are some example policies for restricting access to secrets.

----

Granting read access to one secret 

``` json
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "secretsmanager:GetSecretValue",
        "Resource": "<arn-of-the-secret-the-app-needs-to-access>"
    }
}
```

----

Limiting access to specific, read-only actions 
``` json
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "secretsmanager:Describe*",
            "secretsmanager:Get*",
            "secretsmanager:List*" 
        ],
        "Resource": "*"
    }
}
```

----

**Granting permission to the version of the secret with a certain staging label.**
The following policy, when attached to a user, group, or role, allows the user to use GetSecretValue on any secret with a name that begins with Prod—and only for the version with the staging label AWSCURRENT attached. 
``` json 
{
    "Policy": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "secretsmanager:GetSecret",
                "Resource": "arn:aws:secretsmanager:*:*:secret:Prod*",
                "Condition": { "ForAnyValue:StringEquals": { "secretsmanager:VersionStage": "AWSCURRENT" } }
            }
        ]
    }
}
```