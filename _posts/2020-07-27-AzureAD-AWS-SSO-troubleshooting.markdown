---
layout: post
title:  "Troubleshooting Azure AD to AWS SSO SAML provider setup"
date:   2020-07-27 6:00:00 -0500
categories: aws azure ad active directory sso saml cloud identity o365 office
comments: True
---

### Overview
Well, I just pulled an all nighter. I was originally planning to make some great progress on my studying for the [AWS Certified Developer - Associate](https://aws.amazon.com/certification/certified-developer-associate/) exam, but instead went down a marathon rabbit hole to setup SSO access to the AWS console using Azure AD / O365 identities. This was something I have been wanting to do for a long time.  I know my clients will find great value in extending AD as their single source of identity to the AWS console, CLI, SDKs, etc. I found the two methods below which both enable SAML-based SSO from Azure AD into AWS. There are pros and cons to each one.
**Resource:** [AWS Identity Federation Overview Docs](https://aws.amazon.com/identity/federation/)

### High-level Steps
1.  Setup Enterprise App in Azure AD
1.  Create AWS Federation profile
1.  Save SAML metadata XML files from Azure AD app and AWS profile
1.  Upload SAML metadata XML files to opposite system
1.  Setup role(s) to grant access to federated users
1.  Enable automatic provisioning
1.  Test AD user access

### ~5 hours WASTED
I am ashamed to say that a couple of pound signs "#" were the cause of a lot of problems and extra troubleshooting, leading me to stay up just a little longer because the fix could be just this next step to try... DANGIT! Ok, what now... 3:30AM?! Do I go to bed and get only 3 hours of sleep or just keep going? I kept going and luckily it paid off. I eventually discovered that the Chrome Developer Tools showed the exact error that AWS was complaining about. I had registered for the O365 tenant with my gmail account and for some weird reason, Microsoft decided to put a "#EXT#" in my username. I had been setting it up properly all night long. Did those pound signs really waste 5 hours of precious sleep, or maybe study time?? ARGH!

### AWS IAM Federation
**Resource:** [IAM Federation Guide - Azure Docs](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/amazon-web-service-tutorial)   
To setup the SAML trust between Azure AD and AWS, an Enterprise App needs to be created inside the Azure AD admin panel. The IAM Federation method requires the Azure AD Gallery App  named "Amazon Web Services" which provides a pre-built template to build from. After I imported the AWS SAML metadata files to AD, I setup the IAM SAML role to be granted to the federated users. Yes, you have to create a whole new set of IAM roles to grant these federated users. Yuck! I also created the user and policy to allow the Azure AD provisioning service to connect to IAM and gather the list of SAML roles available to assign to AD users. This is one of the biggest differences between IAM Federation and the AWS SSO method - roles are assigned to users in AD. Also the Azure AD app takes the user straight into the AWS Console. If you want to setup access to multiple AWS accounts within an Organization, you will need to create multiple Apps in Azure AD.

**Pros:**  
1.  No extra hop to the AWS SSO Dashboard. You are just logged in directly to the AWS console
1.  Role assignments are done in AD, the source of identity truth

### AWS SSO
**Resource:** [AWS SSO Federation - AWS Docs](https://aws.amazon.com/blogs/aws/the-next-evolution-in-aws-single-sign-on/)  
The AWS SSO option was a little more intuitive and simple to execute. I used a non-Gallery app in AD which sounds like it would be more difficult, but AWS put the [SCIM](https://en.wikipedia.org/wiki/System_for_Cross-domain_Identity_Management) information front and center so that I could give the AD App the credentials for the provisioning service. Just add AD users or group to the app and the user is eventually provisioned to AWS. Head over to AWS SSO after the users are provisioned to grant them an existing IAM role. The user experience is also cleaner. Once the AD user signs in, they are taken to an AWS SSO dashboard which lists all of the SSO apps and AWS accounts the user was given access to and they can also select which role to assume if they were granted access to more than one role per account. I definitely prefer this method.

**Pros:**  
1.  Intuitive and simple
1.  Can re-use existing AWS roles instead of having to build SAML-specific ones
1.  Clean user experience on AWS SSO Apps Dashboard

### Feedback
Please let me know if you find this helpful, want help setting something like this up for your organization, or want to share any thoughts or feedback. Many [Blessings](https://www.youtube.com/watch?v=Zp6aygmvzM4) to you and yours.

Thanks!  
-D
