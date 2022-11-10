---
layout: post
title:  "RDP to EC2 instances in a private network"
date:   2022-11-10 11:45:00 -0500
categories: aws cloud ec2 rdp systems manager session tunnel iam
---

### Overview

Do you have bastion hosts provisioned for your AWS environment? With AWS Session Manager you really don't need them anymore, #costoptimization. Session manager gives great command line access to your EC2 instances, but the web-based RDP experience provided in AWS Fleet Manager is a bit lacking, especially when using copy & paste. This post gives steps to enable secure RDP to your private Windows EC2 instances using native RDP client tools on your local machine, no public IP or bastion host needed. It uses IAM credentials and a secure tunnel provided by AWS Session Manager via AWS CLI. It's pretty slick. Check it out.

 
### If AWS CLI cannot be installed on Windows machine
 
#### Requirements

- Local Windows machine that needs to RDP to an EC2 instance in a private network
- Linux VM with AWS CLI v2 and the session manager plugin installed
    - AWS CLI configured and setup with the proper credentials
- Remote EC2 Windows instance not directly/publicly accessible with RDP
    - SSM agent version 2.3.672.0 or later is installed

 
#### Steps

1. Identify the instance ID that you want to connect to.
2. On the Linux VM, open the port forwarding session. The local port number will only be accessible within the machine it is established from.

    ```sh
    sudo aws ssm start-session --target i-0eb32a5c3f38dd4ae --document-name AWS-StartPortForwardingSession --parameters '{"portNumber":["3389"], "localPortNumber":["56789"]}'
    ```

3. On the Linux VM, create another SSH tunnel to forward RDP requests from the Windows machine to the previously established Session Manager Port Forwarding session and therefore on to the remote AWS instance in a private network. Be careful with this step. You're opening access to anyone on your network. You should consider changing 0.0.0.0 to something more limited in scope.

    ```sh
    ssh -L 0.0.0.0:3389:localhost:56789 localhost
    ```

4. On the Windows machine, open the RDP client and connect to the Linux VM by IP address or name if it is in DNS. You should be connected to the AWS EC2 instance and prompted for its credentials.
5. As soon as you are done with your RDP connection, stop the SSH tunnel and SSM session, otherwise you may expose your private EC2 instance to unwanted access.

 
### If AWS CLI is installed on Windows Machine

Note: This is not tested but should work. Let me know if this doesn't work.

 
#### Requirements

- Local Windows machine that needs to RDP to an EC2 instance in a private network
    - AWS CLI v2 and the session manager plugin installed
    - AWS CLI configured and setup with the proper AWS credentials
- Remote EC2 Windows instance not directly/publicly accessible with RDP
    - SSM agent version 2.3.672.0 or later is installed

#### Steps

1. Identify the instance ID that you want to connect to.
2. On the local Windows machine VM, open the port forwarding session. The local port number will only be accessible within the machine it is established from.

    ```sh
    aws ssm start-session --target i-0eb32a5c3f38dd4ae --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="56789"
    ```

3. On the Windows machine, open the RDP client and connect to "localhost:56789". You should then be connected to the AWS EC2 instance and prompted for its credentials
4. As soon as you are done with your RDP connection, stop the SSM session, otherwise you may expose your private EC2 instance to unwanted access.

 
### References

<https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-start-port-forwarding>

<https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html>

<https://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/>
