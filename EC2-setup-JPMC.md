###### Setting up EC2 for DRFC

## FINDING YOUR VPC:

From AWS Console, navigate to “VPC”. Once there select “Your VPCs” and find the VPC that is set as default

 ![GitHub Logo](/images/vpc.png)

Save this vpc name and Network ACL name for later.
Click the network ACL and add an inbound rule for ALL traffic on “your IP”/32

 ![GitHub Logo](/images/inbound.png)

Also add an outbound TCP rule for the following:

 ![GitHub Logo](/images/upload_tcp.png)

## CREATING SECURITY GROUP:

From the AWS Console, navigate to “EC2”. Once there, select “Security Groups” on the left hand panel and then  “Create security group”

Create the following configurations:
*	Vpc is the same as your default vac
*	Add inbound rule to allow all traffic for “your IP”/32
*	Add the same outbound rules as below
 
 ![GitHub Logo](/images/inbound.png)

 ![GitHub Logo](/images/outbound.png)
 
* Add the following tag to make sure the security group rules are not automatically deleted

 ![GitHub Logo](/images/auto_delete.png)

## CREATING EC2:

Navigate to EC2 from AWS Console and select “Instances” from the left side panel

 ![GitHub Logo](/images/instance.png)

Click “Launch Instances” and set up the EC2 with the following:

*	Server type: 18.04 ubuntu
*	Instance type: G4dn.4xlarge  (allows 3 workers. Can also try g4dn.2xlarge or other GPU accelerated instances)
*	IAM Role: AmazonSSMRoleForInstancesQuickSetup
*	Key pair: create and save this in a safe location
*	Security Group: The same name as what you made in earlier section on setting up security group
*	Storage: 40GB to root volume

## CONNECTING TO EC2:

On your instances tab in the EC2 console, click the instance you would like to connect to, and then click “connect”

![GitHub Logo](/images/ec2.png)

On the connect screen, copy the command and paste into any client that is able to SSH (I use terminal on Mac)

 ![GitHub Logo](/images/connect.png)
 
If you are able to get in, congratulations! Now run the drfc setup here: https://larsll.github.io/deepracer-for-cloud/installation.html
If you are unable to connect, please contact Tyler Wooten in Slack https://app.slack.com/client/T01D8BNK3M4/C01DKU2FWMU or through email tyler.wooten@jpmchase.com

**TROUBLESHOOTING:**
*If you can't connect:*
* Ensure Security group outbound is open to all
* Assign IAM role to the group

