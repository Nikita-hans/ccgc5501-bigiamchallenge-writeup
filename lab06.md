## Challenge 1: First Flag
## Solution:

1. Create an AWS Account:
I started by creating a free AWS account. For security purposes, it's a good idea to use a separate account specifically for testing, rather than using a production account.

2.Create a Non-Root IAM User:
I created a non-root IAM user with administrator access. This is a best practice, as it helps reduce the risk of exposure if any credentials are compromised.

3. Generate an Access Key for the IAM User:
After creating the IAM user, I generated an access key for the user. This key allows programmatic access to AWS resources through the AWS CLI.

4. Install AWS CLI and Terraform:
I installed AWS CLI and Terraform on my system. These tools are essential for managing AWS resources and automating infrastructure tasks. I made sure the binaries were added to the system path to use them from the terminal.

5. Verify the Installations:
I verified that the installations were successful by running the following commands:
aws help
terraform help
These commands confirmed that both tools were working properly and provided the available options and commands for each tool.

6. Configure AWS CLI:
I configured the AWS CLI with the IAM user's access key and secret key using the command aws configure. It asked for the region (I selected us-west-2), and the output format (I chose json).

7. Verify AWS CLI Configuration:
To ensure that the CLI was configured correctly, I ran the command aws sts get-caller-identity. The output showed the User ID, Account ID, and ARN, confirming that the configuration was correct.

8. Clone the CloudFoxable Repository:
I cloned the CloudFoxable GitHub repository using the command git clone https://github.com/BishopFox/cloudfoxable. This downloaded all the necessary files to my local machine.

9. Navigate to the AWS Directory and Copy the Terraform File:
In the cloned repository, I navigated to the aws directory and copied the example Terraform variables file:
cp terraform.tfvars.example terraform.tfvars
This file holds the configuration settings required for Terraform to work.

10. Initialize Terraform:
I initialized the Terraform setup by running terraform init. This command downloaded the necessary providers and set up the Terraform working directory.

11. Edit the Terraform Variables File:
I opened the terraform.tfvars file and set the AWS profile name to my local AWS profile. This ensures that Terraform uses the correct credentials when interacting with AWS.

12. Run Terraform Plan:
I ran the terraform plan command to check what changes Terraform would make. This allowed me to see the resources that would be created in AWS before actually applying them.

13. Apply Terraform Configuration:
After reviewing the plan, I ran terraform apply to create the resources in AWS. The output provided credentials for a new user, called ctf-starting-user, which were necessary to proceed.

14. Create a New AWS Profile Using Terraform Output:
I used the access keys from the Terraform output to create a new AWS profile called cloudfoxable. This was done by updating the AWS credentials file with the new access keys and setting the region to us-west-2.

15. Set Environment Variables:
I set the following environment variables using the Terraform output to ensure AWS CLI commands would use the correct credentials:
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION

16. Verify New Credentials:
To check if the credentials were set correctly, I ran the command aws sts get-caller-identity --profile cloudfoxable. The output confirmed that the new profile was working as expected.

17. Find the Flag:

After running the Terraform commands, I found the flag in the output provided by Terraform in the format:
FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}
I copied the flag and submitted it in the CTFd platform, which resulted in a success message.

## Reflection

* What was your approach?
My approach was to follow each step methodically. I first set up the AWS CLI and Terraform to ensure that I had the proper tools for interacting with AWS. I paid close attention to the Terraform output and used it to guide my next steps.

* What was the biggest challenge?
The biggest challenge was ensuring that all the tools were installed and configured correctly. It took a bit of time to get the IAM user and profile management set up in the right way, but verifying with aws sts get-caller-identity helped me troubleshoot.

* How did you overcome the challenges?
I used commands like aws sts get-caller-identity to verify the AWS CLI configuration, and I carefully read the Terraform output to ensure that I understood what was being created. Once I figured out where the credentials were located, I used them to configure my AWS profile properly.

* What led to the breakthrough?
The breakthrough came when I noticed that the Terraform output contained the access keys for the ctf-starting-user. This allowed me to create a new profile and proceed with accessing the AWS resources needed to retrieve the flag.
