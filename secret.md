# Challenge 3: Its a Secret

## Solution
### Step 1: Confirm User and Profile
#### First, I confirmed which AWS user and profile I was using by running:

aws --profile cloudfoxable sts get-caller-identity  

The output confirmed that I was using the correct user:

{
    "UserId": "AIDA5FCD6JJN4KLB723MR",
    "Account": "904233110107",
    "Arn": "arn:aws:iam::904233110107:user/ctf-starting-user"
}

### Step 2: Check for Secrets
#### Next, I used CloudFox to search for any secrets in the environment:

cloudfox aws -p cloudfoxable secrets -v2  

The output displayed several secrets, including one that stood out:
/cloudfoxable/flag/its-a-secret  

### Step 3: Identify the Important Secret
#### I noted the secret path:

/cloudfoxable/flag/its-a-secret  

#### CloudFox also saved the command to access the secrets in this file:
[secrets][cloudfoxable] Loot written to /home/n01702697/.cloudfox/cloudfox-output/aws/cloudfoxable-904233110107/loot/pull-secrets-commands.txt

### Step 4: Check Access Permissions
#### To verify if I had permission to access the secret, I ran:

cloudfox aws -p cloudfoxable permissions --principal ctf-starting-user -v2  

The output confirmed that the user ctf-starting-user had the ssm:GetParameter permission for the secret.

### Step 5: Retrieve the Secret Value
#### Since I had the required permission, I retrieved the secret value by running:

aws --profile cloudfoxable --region us-west-2 ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-a-secret  

The output showed the flag:
"Parameter": {
        "Name": "/cloudfoxable/flag/its-a-secret",
        "Type": "SecureString",
        "Value": "FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}",
        "Version": 1,
        "LastModifiedDate": "2025-02-19T00:04:08.883000-05:00",
        "ARN": "arn:aws:ssm:us-west-2:904233110107:parameter/cloudfoxable/flag/its-a-secret",
        "DataType": "text"
    }

### Step 6: Flag Retrieved
The flag was:
FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}

## Reflection
### My Approach:
I followed a step-by-step process. I first confirmed the user, searched for secrets, and verified if I had the required permissions to access the secret. I used CloudFox to find the secrets and check the user’s permissions.

### Biggest Challenge:
The hardest part was making sure the user had the right permissions to access the secret. Without permissions, I would not have been able to retrieve the flag.

### How I Overcame It:
I solved this by running CloudFox’s permission-checking command. Once I confirmed that the user had ssm:GetParameter permission, I accessed the secret.

### Breakthrough Moment:
The key moment was when I identified the secret path from CloudFox’s output. From there, it was just a matter of checking permissions and running the correct AWS command.

### On the blue side, how can the learning be used in future?
This learning will help me in future cloud security assessments or CTF challenges. Knowing how to use CloudFox and AWS CLI to find and access secrets is valuable for both offensive and defensive cloud security tasks. It also highlights the importance of managing secret permissions properly.