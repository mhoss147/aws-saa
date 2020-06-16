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






























