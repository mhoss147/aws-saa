# aws-saa



# -----------------------------------------------------------------

# Docker

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



