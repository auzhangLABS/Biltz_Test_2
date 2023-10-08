# Deployment 4 Documentation

## Purpose:
The purpose of deployment 4 was to set up a monitoring solution for the Amazon EC2 instance running our application. The primary objective was to continuously monitor the real-time metrics of our EC2 instance, which may include CPU, memory, or RAM. This allows us to detect issues early while optimizing EC2 resources, as well as increasing the reliability of our application. Additionally, we configured a notification system that would allow us to be alert of any critical performance issues with our application with the EC2. This deployment motivates us to proactively address server-related issues, ensuring a resilient application.

## Steps:
Before we started, we created our VPC with two available zones. Within each of those available zones, we have one public subnet and one private subnet (2 AZ, 2 public subnets, and 2 private subnets). We're also using a t2.medium EC2 instance within the public subnet of AZ A. This EC2 instance will also have a security group attached to it. To see the VPC diagram, click [here!](https://github.com/auzhangLABS/Deployment4/blob/main/images/vpcdiagram.drawio.png)
#### Replicating an online repository into our repository
- `git clone {online repo url}`: Cloning an online repository into our local workspace. <br>
- `cd repo name`: Going into the repository folder. <br>
- `cd .git`: Going into the .git folder. <br>
- `nano config`: Here we are going to change the config file to push this repo to our remote repository by changing the `[remote = "origin"] url = {our remote repo url}`. <br>
- `git config --global user.name "Your Name"` `git config --global user.email "Your email"`: This allows us to set the global variable to your name and email.
- `git push`: Pushing it to our remote repository. <br>

Keep in mind that once you push, it will prompt you to enter your username and password (token) for your GitHub.

#### Creating a Branch and Making Edits
- `git branch {branch name}`: Creating a branch. <br>
- `git switch {branch name}`: Change into that branch. <br>
- `nano Jenkinsfile`: Changing the Jenkins file within the text editor. <br>
- `git add Jenkinsfile`: Adding Jenkins files to the stage area. <br>
- `git commit -m "you commit message" `: Committing the file with a commit message. <br>
- `git switch main`: Changing back to main. <br>
- `git merge {branch name}`: Merging changes done on {branch name} onto main. <br>
- `git push`: Pushing to our remote repository. <br>

To see the updated Jenkinsfile click [here!](https://github.com/auzhangLABS/Deployment4/blob/main/Jenkinsfile)

#### Download Jenkins and other additional packages.
- `sudo apt-get update`: Update the repository information. <br>
We are also installing a Pipeline Keep Running Step plugin for Jenkins, this would allow the process to keep running even after the Jenkins build has finished.
To see the steps to install Jenkins, Python Virtual Environment, or Python package manager and installer, click [here!](https://github.com/auzhangLABS/c4_deployment3) <br>
- `sudo apt-get install nginx`: Install the NGINX package. <br>
- `sudo nginx -v`: To verify the installation. <br>

Once we downloaded NGINX, we configured our configuration found at this path: `/etc/nginx/sites-enabled/default` to change the port from 80 to 5000 as well as the location block that handles requests to the root location. <br>
![image](https://github.com/auzhangLABS/Deployment4/assets/138344000/c5e78eae-ecf8-43c1-96b1-e86acbc4c746) <br>

Remember that Nginx will be the presentation layer while Gunicorn will be more at the application layer. Nginx would act as a reverse proxy for Gunicorn (route requests). 

#### Installing the monitoring tool onto EC2
To install Cloudwatch, we had to make sure that we had created an IAM role that allowed us to use the CloudWatch agent on Amazon EC2. Here, I'm showing how to use the command line to install the CloudWatch agent on Amazon EC2.
- `wget https://amazoncloudwatch-agent.s3.amazonaws.com/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb`: To download the Cloudwatch agent for the Ubuntu platform. <br>
- `sudo dpkg -i -E ./amazon-cloudwatch-agent.deb`: To install the CloudWatch agent from the Debian package. <br>
- `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard`: Launch a CloudWatch agent using the configuration wizard. <br>
- `cat file_config.json`: To see what you have configured. You must be within the `/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/` folder. <br>

To see what I configured for CloudWatch, click [here!](https://github.com/auzhangLABS/Deployment4/blob/main/file_config.json). Once you finish configuring it, it will show up in metrics within CloudWatch.

#### Running on Jenkins and server performance
For this deployment, we're creating a multi-branch pipeline project. This allows Jenkins to create a set of pipeline projects according to the branches in the repository. From examining the CloudWatch metrics on this EC2 instance, I was able to tell that the T2.medium was able to handle everything installed on it. Additionally, the CPU will increase once we start another build. For this application, we can also use T2.micro; however, it would perform much slower due to the fact that T2.micro has 1 vCPU whereas T2.medium has 2 vCPUs. Additionally, a T2.medium also comes with more memory (RAM) and better network performance for a higher price.

#### Setting Monitoring Alarms
To set up a Cloudwatch alarm, we go into Alarms and create an alarm. Next, you choose the metric you want to track and set the threshold you want the alarm to trigger. <br>
For this example, I choose to set up an alarm that will trigger if one of my CPU usage goes over 75% (keep in mind, T2.medium has 2 CPUs). <br>
![image](https://github.com/auzhangLABS/Deployment4/assets/138344000/09cd2346-8134-4b3c-ba49-2b2e30a593b2)

#### Configure email notifications on Jenkins
To set email notifications on Jenkins, here are the steps I took:
1) Go into Manage Jenkins, then Configure Systems. <br>
2) Scroll down to E-mail Notification. Here is what I put: <br>
![image](https://github.com/auzhangLABS/Deployment4/assets/138344000/ed64328d-ffbb-415a-ad2c-60f9ad637e36) <br>
For the password, I had to go into Google Password to generate an app password token. I then test the notification to make sure it works. <br>
I set my Jenkins email notifications to email me when the pipeline was completed and successful. I did this by adding an additional stage to the Jenkinsfile.
![image](https://github.com/auzhangLABS/Deployment4/assets/138344000/608743a1-aec8-4e5b-836a-a9bb73083ecd) <br>
However, you can set up a Jenkins email however you want.


## System Design Diagram
To view the system design diagram, click [here!](https://github.com/auzhangLABS/Deployment4/blob/main/images/d4.drawio.png)

## Issues and Troubleshooting
When working on this deployment, I encountered an issue when attempting to push my local repository to my remote Github repository. The console would prompt me to enter my credentials, but consistently, I would receive an incorrect credential error. To address this, I double-check my username and token to ensure the accuracy of my credentials. After this, I was still receiving that error. Upon investigating, I soon figured out that I would need
to grant additional permission for my token that would allow full control of my repositories.
![image](https://github.com/auzhangLABS/Deployment4/assets/138344000/d8acc41c-d950-4f5a-8eb1-fc9abca37b0c) <br>
Another issue I ran into was creating a CloudWatch agent on my instance. I fixed this by modifying the IAM to allow the instance to create a CloudWatch agent.


## Optimization:
I would try to optimize this deployment by implementing CloudWatch to trigger an auto-scaling group. This would allow automatic EC2 scaling, depending on the usage of the instance.


























