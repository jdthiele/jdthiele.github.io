---
layout: post
title:  "Troubleshooting Azure AD to AWS SSO SAML provider setup"
date:   2020-07-27 6:00:00 -0500
categories: aws azure ad active directory sso saml cloud identity o365 office
---

### Overview
Well, I just pulled an all nighter. I was originally planning to make some great progress on my studying for the [AWS Certified Developer - Associate](https://aws.amazon.com/certification/certified-developer-associate/) exam, but instead went down a marathon rabbit hole to setup SSO access to the AWS console using Azure AD / O365 identities. This was something I have been wanting to do for a long time because I know most of my clients will find value in extending AD as their source of identity to the AWS console, CLI, SDKs, etc. There are two different methods to enable SAML-based SSO from Azure AD into AWS.

### High-level Steps
1.  Setup Enterprise App in Azure AD
1.  Create AWS Federation profile
1.  Save SAML metadata XML files from Azure AD app and AWS profile
1.  Upload SAML metadata XML files to opposite system
1.  Setup role(s) to grant access to federated users
1.  Enable automatic provisioning
1.  Test AD user access

### Azure AD
To setup the SAML trust between Azure AD and AWS, an Enterprise App needs to be created inside the Azure AD admin panel. 

### AWS IAM Federation
The IAM Federation method requires that the Azure AD app 

### AWS SSO

 

### Other Resources
[AWS Identity Federation Overview Docs](https://aws.amazon.com/identity/federation/)
[IAM Federation Guide - Azure Docs](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/amazon-web-service-tutorial)
[AWS SSO Federation - AWS Docs](https://aws.amazon.com/blogs/aws/the-next-evolution-in-aws-single-sign-on/)
