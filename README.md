# Biltz 2

## Purpose:
This blitz test aimed to assess how our server would handle higher requests. We will observe and identify ways to allow our server to handle a significant increase in requests to our server.

## Problem:
Our application has become very popular and is growing in demand. We need to ensure at least 14,000 users can access our application anytime. To test this, we will have the QA engineer send 14,000 requests to our server and record the results. 

## Original Test result:
Our QA Engineer sent 14,000 requests to the server and reported that 8 users were unable to reach our application.

## Solution:
### Vertical Scale the instance
#### Method 1 (Simple): 
1. Stop the instance
2. Click on Actions
3. Go into Instance Setting -> Change instance type
4. Change the instance to a t2.xlarge <br>
![image](https://github.com/auzhangLABS/Biltz_Test_2/assets/138344000/3801992b-7211-48c9-b265-0a146f1fcc84)
<br>

#### Method 2 (Complex however allows you to change instance configuration): 
1. Stop the instance
2. Click into instance information and go into storage.
3. Find the Volume ID and click on it
4. Detech it from the T2.medium instance (remember the instance ID or name it)
5. Start a new instance with the same OS, keypair, VPC, and subnet configuration as the instance before.
6. Choose the instance type to be t2.xlarge and launch the instance.
7. Detach the default volume on the t2.xlarge instance.
8. Attach the volume from the detached t2.medium to the t2.xlarge volume.
9. Use /dev/sda1 as the device name for the root volume.
