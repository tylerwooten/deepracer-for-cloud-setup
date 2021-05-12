
To easily set up all of the correct JPMC resources at one time (VPC connection, allowlisting your machines IP, EFS (Elastic File System) setup, etc. ), Esa Laine has created a cloudformation stack that lets you complete the entire setup in one command.  Download the zip folder containing the following relevant files.

https://github.com/EsaLaine/deepracer-templates

After downloading the files, from your AWS Sandbox launch "CloudShell" and upload the following files from that folder:

# UPLOAD FILES:
- Actions > Upload file
- upload create-base-resources.sh (have to upload one at a time)

## Relevant files:
- create-base-resources.sh
- base-resources.yaml
- create-instance.sh
- standard-instance.yaml 
- create-standard-instance.sh

# Set environment variables:
Set the following variables up:  
BASE_STACK_NAME = the name of your base stack containing only letters & numbers  
YOUR_MACHINES_IP = the IPV4 of the machine you are using to access the ec2 instance (https://whatismyip.host/)   
RULE_NUMBER = pick a random 4 digit number that does not collide with your VPC ACL's rule numbers  
EC2_STACK_NAME = the name of your base stack containing only letters, numbers, and dashes  

EXAMPLE:
```
export BASE_STACK_NAME=drfcBaseStack
export YOUR_MACHINES_IP=111.111.1.111
export RULE_NUMBER=1234
export EC2_STACK_NAME=drfcEC2Stack
```


# BASE RESOURCES SETUP:
(only need to run this once per account/machine accessing EC2 instance)
- Run ```bash create-base-resources.sh $BASE_STACK_NAME $YOUR_MACHINES_IP $RULE_NUMBER```


# CREATE STANDARD EC2 INSTANCE:
- Run ```bash create-standard-instance.sh $BASE_STACK_NAME $EC2_STACK_NAME```

you can go to "Cloudformation" to watch the progress of your stack creation and see if any errors occurred in the "Events" tab.

# First Run:
- Access your EC2 instance using SSM from EC2 console page
- run ```sudo su ubuntu``` to switch users
- run ```bin/bash``` to enter bash shell prompt
- run ```source bin/activate.sh``` to get access to dr-commands
- run ```cd /home/ubuntu/deepracer-for-cloud``` to navigate to the deepracer directory
- setup s3 bucket in system.env and run.env (e.g. DR_LOCAL_S3_BUCKET=drfcbasestackdmh1-bucket-111111, DR_UPLOAD_S3_BUCKET=drfcbasestackdmh1-bucket-111111/upload)
- follow “First Run” on this page https://aws-deepracer-community.github.io/deepracer-for-cloud/installation.html 

After you have your EC2 instance set up, when you are done training you should "stop" your instance and not "terminate" it. You will have to run "CREATE STANDARD EC2 INSTANCE" again if you terminate your instance. You will not be charge the hourly rate for a stopped instance.

