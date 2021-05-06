
To easily set up all of the correct JPMC resources at one time (VPC connection, allowlisting your machines IP, EFS (Elastic File System) setup, etc. ), Esa Laine has created a cloudformation stack that lets you complete the entire setup in one command.  Download the correct files here:
https://github.com/EsaLaine/deepracer-templates

Relevant files:
- create-base-resources.sh
- base-resources.yaml
- create-instance.sh
- standard-instance.yaml 
- create-standard-instance.sh


After downloading these files, launch AWS CloudShell in your AWS console.

BASE RESOURCES SETUP (only need to run this once per account/machine accessing EC2 instance):
- Actions > Upload file
- upload create-base-resources.sh and upload base-resources.yaml
- Run “bash create-base-resources.sh BASE_STACK_NAME YOUR_MACHINES_IP RULE_NUMBER”
BASE_STACK_NAME = the name of your stack. This can be whatever you want
YOUR_MACHINES_IP = the machine you are using to access the ec2 instance 
RULE_NUMBER = pick a random 4 digit number that does not collide with your VPC ACL's rule numbers



CREATE STANDARD EC2 INSTANCE (run this to create a fresh EC2 instance):
- from AWS Cloudshell
- Actions > Upload file
- Upload standard-instance.yaml and create-standard-instance.sh
- Run “bash create-standard-instance.sh BASE_STACK_NAME EC2_STACK_NAME”
BASE_STACK_NAME = stack name from above
EC2_STACK_NAME = the name of your stack. This can be whatever you want


Access your EC2 instance using SSM from EC2 console page and follow “First Run” on this page https://aws-deepracer-community.github.io/deepracer-for-cloud/installation.html 

If you don’t have “dr-…” commands, run ./bin/prepare.sh

