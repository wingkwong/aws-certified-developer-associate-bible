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
