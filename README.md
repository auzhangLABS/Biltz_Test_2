# Biltz 2

## Purpose:
This blitz test aimed to assess how our server would handle higher requests. We will observe and identify ways to allow our server to handle a significant increase in requests to our server.

## Problem:
Our application has become very popular and is growing in demand. We need to ensure at least 14,000 users can access our application at any time. To test this, we will have the QA engineer send 14,000 requests to our server and record the results.

## Original Test Results:
Our QA engineer sent 14,000 requests to the server and reported that 8 users could not reach our application.

## Solution:
#### Vertical scale the instance can increase the power of the existing instance to meet the rise in demand. We are scaled from a t2.medium to a t2.xlarge, which is an increase from 2 vCPUs to 4 vCPUs, an increase from 4 Mem to 16 Mem, and an enhanced network performance. Just to let you know, there are many other instances where this will work, including T3, T4, M4, etc. Here are some comparisons of the T2 instances: <br>
![image](https://github.com/auzhangLABS/Biltz_Test_2/assets/138344000/22b18950-6b48-4934-9042-1171c90b2bd8)

#### Method 1 (Simple):
1. Stop the instance
2. Click on Actions
3. Go into Instance Setting > Change instance type
4. Change the instance to a t2.xlarge <br>
![image](https://github.com/auzhangLABS/Biltz_Test_2/assets/138344000/3801992b-7211-48c9-b265-0a146f1fcc84)
<br>

#### Method 2 (complex, however, allows you to change instance configuration):
1. Stop the instance
2. Click on instance information and go into storage.
3. Find the Volume ID and click on it
4. Detech it from the T2.medium instance (remember the instance ID or name it)
5. Start a new instance with the same OS, keypair, VPC, and subnet configuration as the instance before (this can be modified using this method)
6. Choose the instance type to be t2.xlarge and launch the instance.
7. Detach the default volume on the t2.xlarge instance.
8. Attach the volume from the detached t2.medium to the t2.xlarge volume.
9. Use /dev/sda1 as the device name for the root volume.

#### After changing the instance, I had to make sure Jenkin was run though to ensure that the application was working

## Vertical Scaling System Design:
To see the Vertical Scaling System Design, click [here!](https://github.com/auzhangLABS/Biltz_Test_2/blob/main/Biltz2_VerticalScaling.drawio.png)

## Current Test Results:
Our QA engineer sent 14,000 requests to the server and reported that all the users could reach our application without any errors.

## Issues and Troubleshooting
With method 2, an issue I faced was with the Cloudwatch agent. When attaching the volume block to the instance and then running it, I figured out that Cloudwatch was not tracking it. I resolved this by modifying the IAM role (CloudWatchAgentServerRole) for this instance and running this command: <br>
`sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json`. <br>
This config.json file should already exist and I was able to find it in Cloudwatch

## Optimization:
I would implement a way to monitor your server and have the system auto-scale your instance horizontally or vertically, depending on business needs.
