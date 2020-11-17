+++
title = "Automatic Rotation"
chapter = true
weight = 40
+++

One advantage of [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) over **SSM Parameter Store** the its ability to automatically rotate secrets.  In this section we will look at the rotation options.

Find then select your secret in the [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/home#/listSecrets)

Scroll down to the **Rotation configuration** section and click the **Edit rotation** button.

{{% img "EditRotation.png" "Credentials" %}} 

On the Edit rotation dialog click enable and give the new AWS Lamdba function a name (e.g. RDSMySQLpasswordRotation).

{{% img "Rotation.png" "Credentials" %}} 

Upon return to the secret details page you should see that rotation is enabling.  

{{% img "Enabling.png" "Credentials" %}} 