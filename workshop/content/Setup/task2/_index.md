+++
title = "Setup Resources"
chapter = true
weight = 20
+++

Using [AWS Cloudformation](https://console.aws.amazon.com/cloudformation/home#/stacks) launch a stack to configure your lab enviorment using the button below.

| Launch Template  | Download Template |
| ----- | ----- |
| <a href="https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=rds-pg-labs&templateURL=https://pg-rds-lab-cloudformation.s3.amazonaws.com/nested/rds-pg-labs-main.yml" target="_blank"><img src='/lab0-prerequisites/task2/cloudformation-launch-stack.png' alt="Launch Stack" style="float:left"></a> | [rds-pg-labs-main.yml](https://pg-rds-lab-cloudformation.s3.amazonaws.com/nested/rds-pg-labs-main.yml) |
<br>
<br>

Once your stack is created goto [AWS Cloud9](https://console.aws.amazon.com/cloud9/home) and Open the IDE.  Using the (+) open a terminal window.  Leave this tab open for the duration of the workshop we will be using it in future sections.

{{% img "labstack-Cloud9-IDE-AWS-Cloud9.png" "Console" %}}  

In the new terminal, paste and run these commands

```
# Install required tools for PostgreSQL
sudo yum install -y jq
sudo yum update -y mysql

# Setup DB Connectivity
AWSREGION=`aws configure get region`
MYSQL_HOST=`aws rds describe-db-instances --region $AWSREGION --query 'DBInstances[*].Endpoint.Address' | jq -r '.[0]'`
DBUSER=`aws rds describe-db-instances --region $AWSREGION --query 'DBInstances[*].MasterUsername' | jq -r '.[0]'`   
MYSQL_PWD=Test1234!
```

{{% notice note %}}
Your database was created with the MasterUser's password test to **"Test1234!"**  We will change this in the coming section.
{{% /notice %}}  

```
# Connect to MySQL RDS Database
mysql mylab
```

{{% notice info %}}
To exit mysql shell type **\q**
{{% /notice %}}  

