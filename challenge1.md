# Challenge 1

## Challenge Statement
We all know that public buckets are risky. But can you find the flag?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I can list the objects under the files/ directory in the bucket, because of ListBucket permission.
I can download any file from the bucket because the GetObject permission is granted for all objects in the bucket.


* What don't I have access to?
I am not able to write or upload files to the bucket. The policy has only granted GetObject and ListBucket permissions. So, no permission for any action such as PutObject or DeleteObject.
I am not allowed to list objects outside of the files/ directory because the StringLike condition limits the listing to this prefix.

* What do I find interesting?
The most interesting aspect of this policy is the public access granted via the Principal: "*". This means anyone can interact with the S3 bucket and download its contents. As someone new to AWS CLI, I find this really interesting because it shows how easy it can be for anyone to access a public bucket and get its data. However, this is a significant security risk if sensitive information is stored there.
This policy only grants ListBucket for files under the files/ prefix, which is a bit of a safeguard, but it still allows full access to objects within that directory. It seems like a small protection, but it doesn't fully secure the bucket from being used by anyone who knows the name of the bucket or its files.


* etc
```

## Solution
Detailed steps to the flag

First, I will list the files in the files/ folder to see what is there. I can do this using the AWS CLI:
aws s3 ls s3://thebigiamchallenge-storage-9979f4b/files/

Once I see the list of files, I can download them. I will use the following command:
aws s3 cp s3://thebigiamchallenge-storage-9979f4b/files/<file-name> ./

After downloading the files, I will open them to check if any of them contain the flag.

## Reflection
* What was your approach?
I started by reading the IAM policy to understand what I could and couldn’t do. Then, I listed the files in the files/ folder and downloaded them to search for the flag.

* What was the biggest challenge?
The biggest challenge for me was fully understanding the IAM policy, particularly the condition on listing only files in the files/ folder. I had to pay close attention to the details of the policy to make sure I was following the right steps.

* How did you overcome the challenges?
I took my time to carefully read the IAM policy and tested the permissions using the AWS CLI. By trying the commands step-by-step, I confirmed that I had the right access to list and download files.


* What led to the break through?
Once I realized that the bucket was publicly accessible, I understood that I could freely download files. This helped me focus on listing and downloading the files from the files/ directory and searching for the flag.


* On the blue side, how can the learning be used to properly defend the important assets? 
This challenge taught me the dangers of opening S3 buckets to the public. To secure sensitive data, one should:
Make sure public access is blocked unless necessary.
Use restrictive IAM policies that limit access to only specific users or roles.
Regularly scan and update your permissions to not leak sensitive information.
Use encryption to keep data safe, both when it’s stored and when it’s being transferred.


