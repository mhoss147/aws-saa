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






























