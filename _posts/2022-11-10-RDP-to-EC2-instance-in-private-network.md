---
layout: post
title:  "RDP to EC2 instances in a private network"
date:   2022-11-10 11:45:00 -0500
categories: aws cloud ec2 rdp systems manager session tunnel iam
---

### Overview

This process will allow you to RDP to a Windows EC2 instance that is in a private AWS network with no public IP address using IAM credentials and a secure tunnel provided by AWS Session Manager.

 
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

3. On the Linux VM, create another SSH tunnel to forward RDP requests from the Windows machine to the previously established Session Manager Port Forwarding session and therefore on to the remote AWS instance in a private network.

    ```sh
    ssh -L 0.0.0.0:3389:localhost:56789 localhost
    ```

4. On the Windows machine, open the RDP client and connect to the Linux VM by IP address or name if it is in DNS. You should be connected to the AWS EC2 instance and prompted for its credentials

 
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
 
### References

<https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#sessions-start-port-forwarding>

<https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html>

<https://aws.amazon.com/blogs/aws/new-port-forwarding-using-aws-system-manager-sessions-manager/>
