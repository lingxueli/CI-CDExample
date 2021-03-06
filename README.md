CI/CD example

This is a general process of CI/CD using the tools below:
	AWS
	Docker
	Jennkins
	Maven
	Git


launch AWS instance:

	Settings:
	Amazon Machine Image (AMI):
		Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type

	Security group (i.e. firewall rules):

		turn on TCP port 8080 allowing any sources

	Key Pair:

		public & private keys for SSH connection


Install and Run docker:

	1. ssh to AWS instance from localhost

	2. install docker
		[ec2-user@ip-172-31-44-247 ~]$ sudo yum install docker

		[ec2-user@ip-172-31-44-247 ~]$ docker --version
		Docker version 18.09.9-ce, build 039a7df

		[ec2-user@ip-172-31-44-247 ~]$ sudo usermod -a -G docker $USER


	3. start docker

	[ec2-user@ip-172-31-44-247 ~]$ sudo service docker start
	Starting cgconfig service:                                 [  OK  ]
	Starting docker:        .                                  [  OK  ]


Install and run Jenkins:

	1. install docker image of Jenkins:

	https://hub.docker.com/_/jenkins/


	[ec2-user@ip-172-31-44-247 ~]$ docker run --rm -u root -d --name myjenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home jenkins
	3e7fc9d51959f669b28474e5d5be4f9a39df27f0552acc204871b3e509aff4ca 
	
	2. run a shell into Jenkins

	[ec2-user@ip-172-31-44-247 ~]$ docker ps -a
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
	3e7fc9d51959        jenkins             "/bin/tini -- /usr/l…"   2 minutes ago       Up 2 minutes        0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   myjenkins

	[ec2-user@ip-172-31-44-247 ~]$ docker exec -it myjenkins bash
	root@3e7fc9d51959:/#                                                                                                                                                                                      
	3. install maven in Jenkins shell


Set up Jenkins pipeline:

	1. generate key pair from Jenkins shell
 
	root@3e7fc9d51959:/# ssh-keygen -t rsa


	2. add its private key to Jenkins through GUI: http://[aws instance public DNS]:8080/

	3. add its public key to Github project


	4. add Github project as a Jenkins job and build it


Now everytime there is a change or commit from git, it will trigger an automatic build through Jenkins pipeline.