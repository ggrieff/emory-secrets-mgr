+++
title = "Setup Resources"
chapter = true
weight = 2
+++

Using [AWS Cloudformation](https://console.aws.amazon.com/cloudformation/home#/stacks) launch a stack to configure your lab enviorment using the button below.

| Launch Template  | Download Template |
| ----- | ----- |
| <a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateUrl=https%3A%2F%2Fggrieff-customer-share.s3.amazonaws.com%2Fbuild.yml&stackName=labstack&param_TemplateName=labstack" target="_blank"><img src='https://master.dlwhmwrl92mne.amplifyapp.com/setup/task2/cloudformation-launch-stack.png' alt="Launch Stack" style="float:left"></a> | [**build.yml**](https://ggrieff-customer-share.s3.amazonaws.com/build.yml) |

Once your stack is created goto [AWS Cloud9](https://console.aws.amazon.com/cloud9/home) and Open the IDE.  Using the (+) open a terminal window.  Leave this tab open for the duration of the workshop we will be using it in future sections.

{{% img "labstack-Cloud9-IDE-AWS-Cloud9.png" "Console" %}}  

In the new terminal, copy and run these commands

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
mysql -h $MYSQL_HOST -u $DBUSER -p`echo $MYSQL_PWD` mylab                                                      
```

Once connected to the database, you can use the below command to create a table with some sample data.

``` sql
CREATE TABLE USERINFO (
   USERID  NUMERIC(9) PRIMARY KEY,
   EMAIL_ADDR  VARCHAR(100),
   FIRST_NAME VARCHAR(100),
   LAST_NAME VARCHAR(100),
   ACTIVE BOOLEAN
);

INSERT INTO USERINFO VALUES (1,'fred@bedrock.corp','Fred','Flintstone',true);
INSERT INTO USERINFO VALUES (2,'wilma.flintstone@gmail.com','Wilma','Flintstone',true);
INSERT INTO USERINFO VALUES (3,'','Pebbles','Flintstone',false);
INSERT INTO USERINFO VALUES (4,'barney@bedrock.corp','Barney','Rubble',true);
INSERT INTO USERINFO VALUES (5,'Betty0724@yahoo.com','Betty','Rubble',true);
INSERT INTO USERINFO VALUES (6,'','BamBam','Rubble',false);
```

{{% notice info %}}
To exit mysql type **\q**
{{% /notice %}}  