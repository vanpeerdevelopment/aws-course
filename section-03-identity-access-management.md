# Section 3 - Identity Access Management

## 12. IAM 101 (__exam__)
#### What
- Manage users and level of access to the AWS console
- Identity federation: use AD, Facebook, ... as identity providers
- Temporary access for users/devices and services

#### Terms
- User: end user
- Group: collection of users with same permissions, e.g. admins, finance, ...
- Role: assign them to AWS resources without having to assign usernames and passwords to the AWS services. E.g. give an EC2 instance the role the access S3.
- Policies: documents (JSON) defining one or more permissions and can be applied to users, groups or roles


## 13. IAM - Lab
- Customize [user sign-in link](https://dietervp.signin.aws.amazon.com/console) to make it easier to remember.
- IAM does not have a region. Users, groups, roles and policies will be available in all regions.
- The root account is the email address used during sign-up. This account has unlimited access.

#### Security status
- Delete your root access keys because they provide unrestricted access to your AWS resources. Use IAM user access keys or temporary security credentials.
- Activate multi-factor authentication (MFA) on your root account.
- Create individual IAM users and give them only the permissions they need. Don't use your root account for day-to-day interaction with AWS. 
- Use groups to assign permissions
- Apply an IAM password policy


## 14. Creating A Billing Alarm - Lab
- Enable billing alerts in _My Billing Dashboard > Preferences_
- Setup a billing alarm in CloudWatch


## 15. IAM Summary
- Summary