# Identity Access Management (IAM)

### IAM 101
- IAM allows you to manage users and their level of access to the AWS Console.
- IAM gives you
  - Centralized control of your AWS account
  - Shared Access to your AWS account
  - Granular Permissions
  - Identity Federation (including Active Directory, Facebook, Linkedin etc)
  - Multifactor Authentication
  - Provide temporary access for users/devices and services where necessary
  - Allows you to set up your own password rotation policy
  - Integrates with many different AWS services
  - Supports PCI DSS Compliance
- Critical Terms:
  - Users - End Users
  - Groups - A collection of users under one set of permissions
  - Roles - You create roles and can then assign them to AWS resources
  - Policies - A document that defines one (or more permissions)

### Security Token Service (STS)

- Federation (typically Active Directory)
  - Uses Security Assertion Markup Language (SAML)
  - Grants temporary access based off the users Active Directory credentials. Does not need to be a user in IAM
  - Single sign on allows users to log in to AWS console without assigning IAM credentials
- Federation with Mobile Apps
  - User Facebook/Amazon/Google or other OpenID providers to log in
- Cross Account Access
  - Let users from one AWS account access resources in another
- Key Terms:
  - Federation: combining or joining a list of users in one domain (such as IAM) with a list of users in another domain (such as Active Directory, Facebook etc)
  - Identity Broker: a service that allows you to take an identity from point A and join it (federate it) to point B
  - Identity Store: Services like Active Directory, Facebook, Google etc
  - Identity: a user of a service like Facebook etc
- Scenario:
  - You are hosting a company website on some EC2 web servers in your VPC. Users of the website must log in to the site which then authenticates against the companies active directory servers which are based on site at the companies head quarters. Your VPC is connected to your company HQ via a secure IPSEC VPN. Once logged in the user can only have access to their own S3 bucket. How do you set this up.
- Solution:
  1. Employee enters their username and password
  2. The application calls an Identity Broker. The broker captures the username and password
  3. The Identity Broker uses the organization's LDAP directory to validate the employee's Identity
  4. The Identity Broker calls the new GetFederationToken functions using IAM credentials. The call must include an IAM policy and a duration (1 to 36 hours) along with a policy that specifies the permissions to be granted to the temporary security credentials
  5. The Security Token Service confirms that the policy of the IAM user making the call to GetFederationToken gives permission to create new tokens and then returns four values to the application: An access key, a secret access key, a token and a duration (the token's lifetime)
  6. The Identity Broker returns the temporary security credentials to the reporting application
  7. The data storage application uses the temporary security credentials(including the token) to make requests to Amazon S3
  8. Amazon S3 uses IAM to verify that the credentials allow the requested operation on the given S3 bucket and Key
  9. IAM provides S3 with the go-ahead to perform the requested operation
- In the Exam
  1. Develop an Identity Broker to communicate with LDAP and AWS STS
  2. Identity Broker always authenticates with LDAP first, Then with AWS STS
  3. Application then gets temporary access to AWS resources

### Active Directory Federation
- Questions:
  - Can you authenticate with Active Directory?
    - Yes. It's using SAML
  - Whether or not you're authenticating to Active Directory first, and then given a temporary security credential or if you get the temporary security credential first which is then authenticated against Active Directory
    - authenticate against Active Directory first,then you would be assigned the temporary security credential

### Web Identity Federation
- Authentication with Identity Provider
  - Verify identity with Facebook
  - Receive an access token
- Obtain temporary security credential
  - Use the access token to obtain temporary security credentials by making an AssumeRoleWithWebIdentity request.
- Access AWS Resources

### IAM Summary
- IAM consists of the following:
  - Users
  - Groups (A way to group our users and apply policies to them collectively)
  - Roles
  - Policy Documents
- IAM is universal. It does not apply to regions at this time
- The 'root account' is simply the account created when first setup your AWS account. It has complete Admin access.
- New Users have NO permissions when first created
- New Users are assigned Access Key ID & Secret Access Keys when first created
- These are not the same as a password, and you cannot use the Access Key ID & Secret Access Key to Login in to the console. You can use this to access AWS via the APIs and Command Line however.
- You only get to view these once. If you lose them, you have to regenerate them. So save them in a secure location.
- Always setup Multifactor Authentication on your root account.
- You can create and customize your own password rotation policies

### Quiz
- Which statement best describes IAM?
  - [x] IAM allows you to manage users, groups and roles and their corresponding level of access to the AWS Platform.
  - [ ] IAM allows you to manage users passwords only. AWS staff must create new users for your organisation. This is done by raising a ticket.
  - [ ] IAM allows you to manage permissions for AWS resources only.
  - [ ] IAM stands for Improvised Application Management and it allows you to deploy and manage applications in the AWS Cloud.

- Which is NOT a feature of IAM?
  - [ ] Centralised control of your AWS account
  - [ ] Integrates with existing active directory account allowing single sign on
  - [ ] Fine-grained access control to AWS resources
  - [x] Allows you to setup biometric authentication, so that no passwords are required

- EC2 instances can have credentials stored on them so that the instances can access other resources (such as S3 buckets) AND AWS recommends that you do this instead of assigning roles.
  - [ ] True
  - [x] False

- What is the name of the service to allow users to use their social media account to gain temporary access to the AWS platform?
  - [ ] Active Directory Authentication Services
  - [ ] Web Confederation Services
  - [x] Web Identity Federation
  - [ ] Facebook Sign In Service

- What is the API call used to obtain temporary security credentials when authenticating using Web Identity Federation?
  - [ ] GetRoleWithWebIdentity
  - [x] AssumeRoleWithWebIdentity
  - [ ] GetRole
  - [ ] AssumeRole

- What is the name of the API call to request temporary security credentials from the AWS platform when federating with Active Directory?
  - [ ] GetSAMLRole
  - [ ] ShowMeTheSAML
  - [x] AssumeRoleWithSAML
  - [ ] CovertRoleToSAML

- When using active directory to authenticate to AWS what are the correct steps performed?
  - [ ] 1) The user navigates to the AWS console, 2) The user enter in their active directory single sign on credentials in to AWS, 3) The user's web browser receives a SAML assertion from AWS,  4) The user is then able to access the AWS Console.
  - [x] 1) The user navigates to ADFS webserver, 2) The user enter in their single sign on credentials, 3) The user's web browser receives a SAML assertion from the AD server, 4) The user's browser then posts the SAML assertion to the AWS SAML end point for SAML and the AssumeRoleWithSAML API request is used to request temporary security credentials. 5) The user is then able to access the AWS Console.
  - [ ] 1) The user navigates to ADFS webserver, 2) The user enter in their single sign on credentials, 3) The user's web browser receives a SAML assertion from the AD server, 4) The user's browser then posts the SAML assertion to the AWS SAML end point for SAML and the GiveUserSAMLAccess API request is used to request temporary security credentials. 5) The user is then able to access the AWS Console.
  - [ ] Federating with Active Directory is not possible with AWS.

- SAML stands for Security Assertion Markup Language.
  - [x] True
  - [ ] False

- The AWS sign-in endpoint for SAML is https://signin.aws.amazon.com/saml
  - [x] True
  - [ ] False

- When using Web Identity Federation to allow a user to access an AWS service (such as an S3 bucket) what is the correct order of steps?
  - [x] 1) A user authenticates with facebook first. They are then given an ID token by facebook. An API call called AssumeRoleWithWebIdentity is then used in conjunction with the ID token. A user is then granted temporary security credentials.
  - [ ] 1) A user logs in to the AWS platform using their facebook credentials. 2) AWS authenticate with facebook to check the credentials. 3) Temporary Security Access is granted to AWS.
  - [ ] Users cannot use Facebook credentials to access the AWS platform.
  - [ ] 1) A user makes the AssumeRoleWithWebIdentity API Call. 2) The user is then redirected to facebook to authenticate. 3) Once authenticated the user is given an ID token. 4) The user is then granted temporary access to the AWS platform.
