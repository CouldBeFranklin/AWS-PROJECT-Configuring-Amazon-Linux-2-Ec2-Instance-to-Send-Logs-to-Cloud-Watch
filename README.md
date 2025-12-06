# AWS-PROJECT-Configuring-Amazon-Linux-2-Ec2-Instance-to-Send-Logs-to-Cloud-Watch


## **PROJECT OVERVIEW**
In this project, I created and  configured an Amazon Linux 2 EC2 instance to securely send system and security logs to Amazon CloudWatch.i ran into some errors but I was able to troubleshoot and find the reasons for the errors and solve them.


## The practical skills demonstrated in this project:
-	Cloud monitoring
-	Linux system logging
-	CloudWatch Agent installation and configuration
-	Troubleshooting
-	Security observability
-	AWS IAM and EC2 operations

## Services and Tools Used
-	Amazon EC2 (Amazon Linux 2)
-	Amazon CloudWatch
-	Amazon CloudWatch Agent
-	AWS Identity and Access Management (IAM)

## Architecture Overview
-	An Amazon Linux 2 EC2 instance sends selected system logs to Amazon CloudWatch using the CloudWatch Agent.
-	IAM Role attached to EC2 allows log publishing.
-	CloudWatch automatically creates log groups upon first successful ingestion.
-	Security logs are analyzed using CloudWatch Logs and Metric Filters.

## STEP 1 : EC2 INSTANCE SETUP
-	I launched an Ec2 instance named “EC2WATCH” in us east region in a default VPC
-	I created a security group for the instance with an inbound rule allowing SSH on port 22 from any ipv4 address (just for testing purpose)


![ec2](CW%20IMGS/EC2%20CREATE.jpg)

## STEP 2 : IAM SETUP
-	The Ec2 instance needed permission to be able to publish logs to cloudwatch and so needed a role with the appropriate policy ensuring least privilege attached to it
-	I created a role named “EC2WATCHROLE” and attached the “CloudWatchAgentServerPolicy” policy to it.
-	Finally, I attached the role to Ec2instance

![iam role](CW%20IMGS/EC2%20ROLE.jpg)

## STEP 3: CLOUDWATCH AGENT SETUP
-	From my local machine I SSH’ed into my Ec2 instance using the downloaded key and public address of the instance.
-	I went ahead to do a complete update to the virtual machine.
-	Then I installed the Amazon CloudWatch agent which was successful
-	I modified the configuration file of the CloudWatch agent

![SSH](CW%20IMGS/SSH.jpg)

![AGENT](CW%20IMGS/AGENT%20INSTALL.jpg)

![CONFIG](CW%20IMGS/AGENT%20CONFIG.jpg)


## FIRST ERROR AND TROUBLESHOOT: DIDN’T APPLY THE MODIFIED CONFIG FILE FOR THE AGENT
-	Was excited and went ahead to check the CloudWatch console to see if the log has been updated but it wasn’t.
-	Checked the agent status but it was failing to start
-	Did a little troubleshoot and found out that I only modified the config file but I never applied it to the CloudWatch agent and restart the agent.
-	And so cloudwatch agent couldn't start with no configuration applied. 

![agent failure](CW%20IMGS/agent%20failure.jpg)

## STEP 4:(SOLUTION) APPLICATION OF MODIFIED CONFIG FILE AND CLOUDWATCH AGENT RESTART
-	I went ahead to apply the modified configuration file to the CloudWatch agent.
-	I restarted the agent and confirmed the agent status.
-	Checked the CloudWatch console to see if the log group has appeared but No log group was seen


![CONFIG APPLY](CW%20IMGS/APPLY%20CONFIG%20RESTART.jpg)
![working agent](CW%20IMGS/agent%20working.jpg)

## SECOND ERROR AND TROUBLESHOOT: LOG PATH ERROR
-	I realized that log path I inputted in the agent configuration (var/log/messages) was the path used by Linux and not Amazon Linux 2 used by my instance.
-	I also noticed that my retention days was set to -1

![WRONG PATH](CW%20IMGS/WRONG%20PATH.jpg)
