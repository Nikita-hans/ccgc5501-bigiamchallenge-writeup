# Challenge number 3

## Challenge Statement
Enable Push Notifications, We got a message for you. Can you get it?

## IAM Policy
```json
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
his policy allows me to subscribe to the SNS (Simple Notification Service) topic named TBICWizPushNotifications in the us-east-1 region. The key here is that it enables subscribing using an endpoint that matches the domain @tbic.wiz.io. So, I can subscribe using an email or endpoint that follows this pattern.

* What don't I have access to?
I don't have access to any other actions outside of SNS:Subscribe. For example, I can't publish messages to the SNS topic or perform any other actions on SNS topics. The policy restricts the sns:Endpoint to a very specific pattern, so if my email or endpoint doesn’t match @tbic.wiz.io, I won't be able to subscribe.


* What do I find interesting?
The most interesting part of the policy is the use of the condition StringLike on the sns:Endpoint. This means that it’s looking for a specific format for the endpoint, which is an interesting way of restricting access to only certain users. It also shows how the SNS:Subscribe action can be controlled by conditions, making it possible to limit access based on specific criteria.

* etc
```

## Solution
Check AWS SNS Docs:
I referred to the AWS SNS documentation to find the CLI command for subscribing to a topic.

Modify the Command:
I modified the command to match the IAM policy, setting the Endpoint to the *@tbic.wiz.io format.

Use Request Catcher:
I used a request catcher tool to capture the subscription request.

Subscribe to SNS Topic:
After modifying the command, I successfully subscribed to the TBICWizPushNotifications topic.

Get the Tag (Flag):
I received the push notification with the flag included in the message.



## Reflection
* What was your approach?
My approach was to carefully follow the steps in the AWS SNS documentation and adapt the provided CLI command to meet the conditions in the IAM policy. I modified the command, ensuring that the endpoint matched the required format and then used a request catcher to monitor the subscription process.

* What was the biggest challenge?
The biggest challenge was ensuring that the Endpoint parameter met the specific condition in the IAM policy (*@tbic.wiz.io). It took some trial and error to get the format right and to make sure I was using the correct subscription process.

* How did you overcome the challenges?
I overcame the challenge by carefully reviewing the IAM policy and modifying the CLI command. Using the request catcher tool helped me identify if the request was being sent correctly, and I tested different endpoint formats until I got it right.

* What led to the break through?
The breakthrough came when I realized the importance of the correct endpoint format (*@tbic.wiz.io). Once I modified the command to match this, the subscription request went through, and I received the notification with the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 
This exercise showed me how IAM policies can be used to restrict access based on conditions like endpoint formats. In a real-world scenario, such restrictions can help protect sensitive resources, ensuring that only authorized users with the correct attributes can interact with important assets. This can be an effective method of securing cloud environments and preventing unauthorized access.
