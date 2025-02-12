# Challenge number 4

## Challenge Statement
Admin only?
We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I have access to download and list objects from the S3 bucket, specifically under the files/ prefix. The policy allows s3:GetObject for all principals and s3:ListBucket with a condition that restricts listing to users whose ARN matches the admin user in the policy.

* What don't I have access to?
 I don't have access to list the contents of the bucket without the proper user authentication, particularly the specific admin user ARN that is supposed to be used for listing files. Without this ARN, access to the s3:ListBucket action is denied.

* What do I find interesting?
I find it interesting that while the policy appears to restrict access to only the admin user, the --no-sign-request flag allowed me to bypass some of the restrictions. This was a useful discovery because it allowed me to access the files despite the lack of proper authentication for listing the bucket.

* etc
```

## Solution
I tried to access the list of files using the aws s3 ls command, but it returned an "Access Denied" error.
I investigated the IAM policy and found that the s3:ListBucket permission was conditioned on the aws:PrincipalArn matching arn:aws:iam::133713371337:user/admin.

After realizing that my ARN wasn't listed, I found the --no-sign-request flag, which allows accessing public S3 buckets without signing the request.

I used the command aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request to successfully list the files.

I then used aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - --no-sign-request to copy the flag, which was {wiz:principal-arn-is-not-what-you-think}.

## Reflection
* What was your approach?
My approach was to analyze the IAM policy and understand how the conditions were set for listing files. Once I encountered the access denial, I explored the aws CLI documentation and found the --no-sign-request option to bypass the need for credentials.

* What was the biggest challenge?
The biggest challenge was figuring out how to bypass the access control despite the restrictions set by the IAM policy. The need to match the admin ARN was a major obstacle.

* How did you overcome the challenges?
I overcame the challenge by researching the --no-sign-request flag and applying it to my aws s3 ls and aws s3 cp commands. This allowed me to access the files and retrieve the flag.

* What led to the break through?
The breakthrough came when I stumbled upon the --no-sign-request flag in the AWS CLI documentation. It was the key to bypassing the restrictions and accessing the files without the proper credentials.

* On the blue side, how can the learning be used to properly defend the important assets?
The lesson here is that relying solely on IAM policies to restrict access can be bypassed if the bucket is publicly accessible. Proper access controls should include not just IAM policies but also other layers of security, such as restricting public access and using bucket policies to prevent unauthorized requests. Regularly auditing S3 bucket permissions and access logs can also help identify and mitigate potential vulnerabilities.
