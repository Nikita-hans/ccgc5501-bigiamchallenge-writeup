# Challenge 2

## Challenge Statement
We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong?
Join our queue and get the secret flag.

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
The policy allows listing the contents of the "example-bucket" (s3:ListBucket), and reading objects from the bucket (s3:GetObject).

* What don't I have access to?
The policy denies uploading objects to the bucket (s3:PutObject). So, you can't add new files to the bucket, but you can view and download the existing files.

* What do I find interesting?
It's interesting that the policy gives permissions for some actions like reading files but specifically denies the ability to upload files. This is useful to limit access and prevent unauthorized changes to data.

* etc
```

## Solution
First, I tried to list the bucket contents using the s3:ListBucket permission, and it worked fine. 
Then, I tried downloading objects from the bucket with the s3:GetObject permission, and that also worked since I’m allowed to read the objects. 
Finally, when I tried to upload a file, I got a "permission denied" error because the policy denies the s3:PutObject action. To find the flag, I looked through the objects I could access in the bucket for anything special

## Reflection
* What was your approach?
My approach was to carefully analyze the IAM policy to understand which actions I was allowed and denied. Then I tested each action by interacting with the bucket to see what worked and what didn’t.

* What was the biggest challenge?
The biggest challenge was figuring out what exactly the IAM policy was blocking and why. Sometimes, permissions can be tricky, and it's important to understand how explicit denies work in AWS policies.


* How did you overcome the challenges?
 I overcame this by closely reading the IAM policy and testing each allowed and denied action. Understanding that "Deny" always takes precedence helped me realize what was happening.

* What led to the break through?
The breakthrough came when I figured out that the policy didn’t allow me to upload files but did give me access to read the objects. I started looking for clues in the readable objects, and that’s where I found the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 
I’ve learned that understanding IAM policies and how permissions work is really important for securing cloud resources. By limiting permissions, like in this challenge, I can make sure only authorized users can access or modify sensitive data. Denying actions like uploads helps reduce the risk of malicious users making unwanted changes.
