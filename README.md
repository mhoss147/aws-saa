# aws-saa



# -----------------------------------------------------------------

# Docker - Create a container using docker

- Start a instance

- Download the security key

- Connect to the instance (by right clicking, choose connect) using Putty/SSH


- check if connected to instance

$ ifconfig


- Install docker

[ec2-user@ip-172-31-68-185 ~]$ sudo amazon-linux-extras install docker


- Start docker

$ sudo service docker start


- Give permission for ec2 user

$ sudo usermod -a -G docker ec2-user


- Exit

$ exit


- Reconnect using same key and check if all ready 

$ docker ps


- Download git

$ sudo yum install git


- Clone git file from url

git clone https://github.com/linuxacademy/content-aws-csa2019.git


- Go to the folder

cd content-aws-csa2019/lesson_files/03_compute/Topic5_Containers/Docker/

ls



-  Build a docker image

docker build -t containercat .    (here containercat is the file and (.) indicates the current directory)

docker images

docker images --filter reference=containercat


- Use this image to run a container

docker run -t -i -p 80:80 containercat      (here -p maps the port; port 80 on docker host and :80 is for container)


- Ready to use, copy the public dns to web browser to see :)


- Publish the container in a docker hub

docker login --username mhoss147

enter my password borochelebond


- Generates a complete lidt of images in machine

docker images


- Tag caontainercat image id with your repo

docker tag ef4141e093ea mhoss147/containercat 


- Push all the Layer to docker hub

docker push mhoss147/containercat

# -----------------------------------------------------

# Designing and Building a Custom VPC from Scratch


# Objectives1: Create VPC and Subnet Architecture


From the VPC console
Create a VPC

    Select Your VPCs.
    Click Create VPC, and set the following values:
        labVPC
        10.0.0.0/16
        Amazon Provided IPv6 block
        Default Tenancy
    Click Create.

Create Subnets

Create a three-AZ, three-app tier subnet layout (leaving spaces for a fourth AZ and fourth tier).

    Select Subnets.
    Click Create subnet.
    Enter the following values in order for Name, VPC, Availability Zone, and IPv4 CIDR Block. Don't assign IPv6 block.
        publicA, labVPC, us-east-1a, 10.0.0.0/24
        publicB, labVPC, us-east-1b, 10.0.1.0/24
        publicC, labVPC, us-east-1c, 10.0.2.0/24
        Skip 10.0.3.0/24 as the reserved space for a fourth AZ public subnet
        privateA, labVPC, us-east-1a, 10.0.4.0/24
        privateB, labVPC, us-east-1b, 10.0.5.0/24
        privateC, labVPC, us-east-1c, 10.0.6.0/24
        Skip 10.0.7.0/24 as the reserved space for a fourth AZ private subnet
        dbA, labVPC, us-east-1a, 10.0.8.0/24
        dbB, labVPC, us-east-1b, 10.0.9.0/24
        dbC, labVPC, us-east-1c, 10.0.10.0/24
        Skip 10.0.11.0/24 as the reserved space for a fourth AZ db subnet

10.0.12.0/24, 10.0.13.0/24, 10.0.14.0/24, and 10.0.15.0/24 can be used for the fourth tier in four AZs, but we won't create them for now.


# Objective2: Create Internet Gateway, Public Routing, and Bastion Host


From the VPC console:

    Select Subnets.
    Select publicA, Actions, and Modify auto-assign IP settings.
    Enable Enable auto-assign public IPv4 address.
    Repeat for publicB and publicC.

Configure Internet Gateway

    Select Internet Gateways, and click Create internet gateway.
    Set the name tag as labVPCIGW, and click Create.
    Select the newly created IGW, click Actions and then Attach to VPC.
    Select labVPC and click Attach.

Configure Routing

    Click Route Tables.
    Click Create route table.
    Set the name tag as publicRT and the VPC as labVPC.
    Click Create.

Add Default Public Route

    Select publicRT, click Routes, Edit routes, and Add route.
    Set the destination as 0.0.0.0/0, target as Internet Gateway, and select labVPCIGW.
    Click Add route again, set the destination as ::/0 , Internet Gateway, and select labVPCIGW.
    Click Save routes.
    Click Close.

Associate with Subnets

    Select publicRT, and click the Subnet Associations tab.
    Click Edit subnet associations.
    Select publicA, publicB, and publicC.
    Click Save.

Create a Bastion Host

From the EC2 console:

    Click Launch Instance.
    Choose Amazon Linux 2, check 64-bit (x86), and click Select.
    Choose t3.micro, and click Next: Configure Instance Details.
    Leave all as defaults, except set the subnet to publicB and make sure Auto-assign Public IP is Use subnet setting (Enable).
    Click Next: Add Storage.
    Click Next: Add Tags.
    Add a tag, and set the Key to Name and Value to BastionHost.
    Click Next: Configure Security Group.
    For security group, create a new one with the name and description bastionSG.
    Click Review and Launch.
    Click Launch, select to Create a new key pair, call it vpclab, and click Download Key Pair.
    Click Launch Instances and then View Instances.

Verify Bastion Host Is Working

From the EC2 console:

    When the bastion host has 2/2 status checks, right-click, click Connect, and copy the connection command.
        Linux/macOS users will need to run a chmod 400 vpclab.pem command first to avoid errors.
        Windows users can connect using this as a guide.
    Run the connection command to connect to your bastion host.



# Objective3: Configure Private Internet Connectivity Using NAT Gateway


 From the VPC Console:
Create the NAT Gateways

    Click NAT Gateways and then Create NAT Gateway.
    Set the subnet to publicA.
    Click Create New EIP and then Create a NAT Gateway.
    Click Close.
    Repeat the process for publicB and publicC for a total of three NAT gateways.
    Select each NAT gateway in turn, and make a note of the NAT Gateway ID and which public subnet it's in.

Create Three Private Route Tables

    Click Route Tables.
    Click Create route table.
    Set the name as privateA-RT and VPC as labVPC.
    Click Create and then Close.
    Repeat for privateB-RT and privateC-RT.

Route Table Associations

Do the following for each route table in privateA-RT, privateB-RT, and privateC-RT.

    Select the Subnet Associations tab, click Edit subnet associations, select the db and private subnets in the same AZ.
        privateA-RT = privateA and dbA
    Click Save.
    On the same route table, click Routes, Edit routes, and Add route.
    Set the destination as 0.0.0.0/0, target as NAT Gateway, and select the NAT Gateway ID in the same AZ (in the list you made earlier).
    Click Close.
    Repeat these steps for each route table.


# Objective4: Configure and Test VPC Security




    Note: For more detailed, step-by-step instructions for this objective, please refer to the lab guide (click the Guide tab next to the Video tab).

    Create an App Server instance on the privateA subnet using the same bastion vpclab key.
    Configure security group, only allowing incoming SSH from the bastion security group.
    Log in to the App Server using SSH key forwarding.
    In the AWS console, navigate to VPC > Network ACLs.
    Select the default NACL, and click Edit inbound rules.
    Click Add Rule, and set the following values:
        Rule #: 50
        Type: ALL Traffic
        Source: Your IP address (which you can get by googling "what is my IP" in a new browser tab), and append /32 at the end
        Allow / Deny: DENY
    In the terminal, try to log in to the bastion host.
    In the AWS console, remove the NACL's rule #50 to remove the explicit DENY.
    In the terminal, try connecting to the bastion host again.


# On Windows: When using SSH Key Forwarding, you will need specific configuration (includes PuTTY).

- https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/


- Pageant

To get SSH agent functionality, you can use Pageant, which is available from the PuTTY download page.

Convert your private key from PEM format to PuTTY format using PuTTYGen

In PuTTYGen, choose Conversions > Import Key and select your PEM-formatted private key. 
 
Enter a passphrase and then click Save private key. Save the key as a .ppk file. 

After you convert the private key, open Pageant, which runs as a Windows service.

To import the PuTTY-formatted key into Pageant, double-click the Pageant icon in the notification area and then click Add Key.

After you add the key, close the Pageant Key List window.

Finally, when you are configuring the connections for SSH in PuTTY, check the Allow agent forwarding box and leave the Private key file for authentication field empty.


# -------------------------------------------------------------------

# Implementing VPC Peering on AWS

- Create a VPC Peer

Ensure you are logged in to the AWS account, INSTANCE1, and INSTANCE2 using the cloud_user credentials provided.

    Change the NACL for Public2 Subnet - change ICMP from 0.0.0.0/0 to 10.0.0.0/13
    Create a VPC peer from VPC1 to VPC2
    Accept the VPC peer between VPC1 and VPC2

- Configure Routing

 Ensure the VPC peer is created and active from Task 1:

    Locate the route tables associated with PublicSubnet1 and PrivateSubnet1
    In each - Add a route for the CIDR of VPC2 and the target of the VPC Peer created in Task 1
    Locate the route tables associated with PublicSubnet2 and PrivateSubnet2
    In each - Add a route for the CIDR of VPC1 and the target of the VPC Peer created in Task 1
    Obtain the privateIP for Instance2 and ping it from Instance1

- Create VPC Peer Mesh

 Ensure the VPC Peer from Task 1 is created and active:

    Create and Accept a VPC peer from VPC2 to VPC3
    Locate the route tables associated with PublicSubnet2 and PrivateSubnet2
    In each - Add a route for the CIDR of VPC3 and the target of the VPC Peer created in Task 1
    Locate the route tables associated with PublicSubnet3 and PrivateSubnet3
    In each - Add a route for the CIDR of VPC2 and the target of the VPC Peer created in Task 1
    Edit the NACL associated with the subnet Instance3 is in. Add a INGRESS rule allowing ICMP IPv4 from 10.0.0.0/13
    Edit the NACL associated with the subnet Instance3 is in. Add a EGRESS rule allowing ICMP IPv4 to 10.0.0.0/13
    Ping the privateIP of Instance3 from Instance2 - does it work? Why?

VPC peering isn't transitive. A pair of peers from VPC1 <-> VPC2 and from VPC2 <-> VPC3 does not mean VPC1 and VPC3 can communicate.

    Create and accept a VPC peer from VPC1 to VPC3
    Locate the route tables associated with PublicSubnet1 and PrivateSubnet1
    In each - Add a route for the CIDR of VPC3 and the target of the VPC Peer created in Task 1
    Locate the route tables associated with PublicSubnet3 and PrivateSubnet3
    In each - Add a route for the CIDR of VPC1 and the target of the VPC Peer created in Task 1
    From Instance1 ping the privateIP of Instance3



- DNS Over VPC Peer

Ensure that the VPC peer created in Task 1, the routing from Task 2, and the VPC peer mesh and routing from Task 3 are all active:

    From the EC2 console locate the public DNS name and private DNS name for Instance2
    From Instance1 ping the public hostname of Instance2, and it should return a public IP
    From the VPC peer options between VPC1 and VPC2 enable both DNS resolution check boxes
    If you wait a few minutes and ping the public DNS name of Instance2 from Instance1, what happens?

Enabling DNS support for VPC peers allows the private IP usage to be forced, if applications always use the instance DNS name.


# ----------------------------------------------------------------


# DNS - TLD -FQDN

$ dig +trace 'google.ca'


# Name server - record - routing policy

$ nslookup www.linuxacademy.com


# if you forget to uncheck all 4 permission boxes when creating s3 bucket

When creating a bucket on the Set Permissions window if you forget to uncheck all 4 permission boxes or to make the bucket public, perform the following steps:

    Navigate to the file directly on the Overview tab of the S3 Management Console.
    
    Select the file’s checkbox.
    
    Click Permissions.
    
    Remove the restrictions so that Group is set to Everyone and Read object is set to Yes for Public access. Read object permissions and Write object permissions should not be set.
    

# if you have problem with DNS using dig utility

cloud_user@sorowar0073c:~$ dig cmcloudlab1441.info  (dig yourWebAddress)


- nslookup utility

cloud_user@sorowar0073c:~$ nslookup cmcloudlab1441.info




    
    




















