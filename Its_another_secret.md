# Cloudfoxable Challenge 4 - It's Another Secret
## Solution

### To solve this challenge, I started by creating a new profile in AWS to take on the Ertz role. I made changes to my AWS config file (~/.aws/config) like this:


[profile ertz]
region = us-west-2
role_arn = arn:aws:iam::904233110107:role/Ertz
source_profile = cloudfoxable

### After saving the file, I checked if I could successfully switch to the Ertz role by running this command:
aws --profile ertz sts get-caller-identity

#### The result showed that I had successfully assumed the role:

{
    "UserId": "AROA5FCD6JJN22WBUQNOX:botocore-session-1743099977",
    "Account": "904233110107",
    "Arn": "arn:aws:sts::904233110107:assumed-role/Ertz/botocore-session-1743099977"
}

### Next, I tried to list the secrets that I could access with this profile using the command:

aws --profile ertz secretsmanager list-secrets

#### However, I got an error saying access was denied:

An error occurred (AccessDeniedException) when calling the ListSecrets operation: User: arn:aws:sts::904233110107:assumed-role/Ertz/botocore-session-1743099977 is not authorized to perform: secretsmanager:ListSecrets because no identity-based policy allows the secretsmanager:ListSecrets action

### To find out more about the permissions of the Ertz role, I used the cloudfox tool, just like in previous challenges, but this time I specified the Ertz role:
cloudfox aws -p cloudfoxable permissions --principal ertz -v2

#### The output showed that the Ertz role had permission to perform the ssm:GetParameter action on a secret:
───────────────────────────────┼───────────┤
│ Role │ Ertz │ Managed │ its-another-secret-policy │ Allow  │ ssm:GetParameter │ arn:aws:ssm:us-west-2:904233110107:parameter/cloudfoxable/flag/its-another-secret │ No        │

### From my previous experience, I knew I could use the ssm:GetParameter action to get the secret. So, I ran this command to retrieve the secret value:

aws --profile ertz --region us-west-2 ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-another-secret

### The result was the flag:
"Parameter": {
        "Name": "/cloudfoxable/flag/its-another-secret",
        "Type": "SecureString",
        "Value": "FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}",
        "Version": 1,
        "LastModifiedDate": "2025-02-19T00:04:07.832000-05:00",
        "ARN": "arn:aws:ssm:us-west-2:904233110107:parameter/cloudfoxable/flag/its-another-secret",
        "DataType": "text"
    }

### The secret flag was:
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}

## Reflection
### What was your approach?
I took a step-by-step approach: first, I assumed the role, checked the permissions with the AWS CLI, and then used the available actions to access the secret.

### What was the biggest challenge?
The biggest challenge was understanding the permissions of the Ertz role. At first, I thought I needed access to Secrets Manager, but the error message showed that the role didn't have permission for that action.

### How did you overcome the challenges?
I used the cloudfox tool to check the Ertz role's permissions. This helped me realize that the role was allowed to perform the ssm:GetParameter action, which let me fetch the secret.

### What led to the breakthrough?
The breakthrough came when I realized that the role had permission for the ssm:GetParameter action. By using this action, I was able to retrieve the secret from AWS Systems Manager.

### How can this learning be used in the future?
This challenge showed me how important it is to understand and check role permissions. In the future, I will use tools like cloudfox more effectively to inspect permissions and find hidden secrets or flags faster.

