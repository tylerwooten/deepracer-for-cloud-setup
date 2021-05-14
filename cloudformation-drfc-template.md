
# Download CloudFormation Files

To easily set up all of the correct JPMC resources at one time (VPC connection, allowlisting your machines IP, EFS (Elastic File System) setup, etc.), Esa Laine has created a Cloudformation Stack that lets you complete the entire setup in one command.

https://github.com/EsaLaine/deepracer-templates

Download the zip folder and extract only the following relevant files:
- create-base-resources.sh
- base-resources.yaml
- create-instance.sh
- standard-instance.yaml 
- create-standard-instance.sh

# Upload CloudFormation Files
Login to your AWS Sandbox and launch "CloudShell" to upload those 5 files:

- Actions > Upload file
- Upload create-base-resources.sh (have to upload one at a time)
- ( ditto for the remaining files )

Continue with the following steps in the same CloudShell terminal session...

# Set environment variables
Setup the following variables:
- BASE_STACK_NAME = the name of your base stack containing only letters & numbers  
- YOUR_MACHINES_IP = the IPV4 of the machine you are using to access the ec2 instance (https://whatismyip.host/)   
- RULE_NUMBER = pick a random 4 digit number that does not collide with your VPC ACL's rule numbers (recommended value **3000**)
- EC2_STACK_NAME = the name of your base stack containing only letters & numbers 

Example commands:
```
export BASE_STACK_NAME=drfcBaseStack
export YOUR_MACHINES_IP=111.111.1.111
export RULE_NUMBER=3000
export EC2_STACK_NAME=drfcEC2Stack
```

# Base Resources Setup
(only need to run this once per account/machine accessing EC2 instance)
- Run ```bash create-base-resources.sh $BASE_STACK_NAME $YOUR_MACHINES_IP $RULE_NUMBER```


# Create EC2 Instance
NOTE: This will create an EC2 instance and charge your sandbox $1.20/hr while the ec2 instance is in a "running" state. Always be sure to stop your instance when you are done training. 
- Run ```bash create-standard-instance.sh $BASE_STACK_NAME $EC2_STACK_NAME```

You can go to "CloudFormation" to watch the progress of your stack creation and see if any errors occurred in the "Events" tab.

# Login to Your EC2 Instance
Obtain Linux command shell access to your running EC2 instance using SSM from EC2 console page as follows:
- Open the "EC2" service
- Choose the "Instances (running)" screen
- Select your instance (its name will be the same as the $EC2_STACK_NAME variable that you defined above)
- Click on the "Connect" button
- Choose the "Session Manager" tab (this is SSM)
- **First time only** Click on the "Preferences" link in the help text, then "Edit" to add the following commands to your "Linux Shell Profile":
````
sudo su ubuntu
cd /home/ubuntu/deepracer-for-cloud
source bin/activate.sh
````
- **Optional** The default idle session timeout is 20 minutes which can be annoying. If you wish, increase it to 60 mins in the "general preferences" section
- Click on the orange "Connect" button
- This should open a Linux shell terminal within your browser

# First Run
- setup s3 bucket in system.env and run.env (e.g. DR_LOCAL_S3_BUCKET=drfcbasestackdmh1-bucket-111111, DR_UPLOAD_S3_BUCKET=drfcbasestackdmh1-bucket-111111/upload)
- follow “First Run” on this page https://aws-deepracer-community.github.io/deepracer-for-cloud/installation.html 

After you have your EC2 instance set up, when you are done training you should "stop" your instance and not "terminate" it. You will have to run "CREATE STANDARD EC2 INSTANCE" again if you terminate your instance. You will not be charge the hourly rate for a stopped instance.

