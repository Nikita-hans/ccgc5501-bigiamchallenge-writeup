# CloudFoxable Bastion 100 Challenge Write-up

## Solution - Detailed Steps



1. To enable the Bastion Host, I went to the `terraform.tfvars` file located in the `cloudfoxable/aws/` directory. Add the following line:

bastion_enabled = true

2. Run terraform apply
Next, I ran the following command:
terraform apply

However, I encountered an error because the policy files contained an invalid JSON format. The issue was caused by the USER_IP variable.

3. Identify the Issue with IP Fetching
The problem was with the USER_IP variable, which was supposed to fetch my public IP dynamically using the following code:
data "http" "current_ip" {
  url = "https://ifconfig.me"
}

locals {
  user_ip = var.user_ip != "" ? chomp(var.user_ip) : chomp(data.http.current_ip.body)
}
Instead of getting the expected IP, the response returned was HTML, which broke the JSON format.

4. Fix the IP Issue
To fix this, I manually fetched my public IP by running the following command:
curl ifconfig.me
I then hardcoded the fetched IP directly into the terraform.tfvars file.

5. Rerun terraform apply
After adding my correct public IP, I ran the terraform apply command again. This time, it worked without errors.

6. Install Cloudfox
Since I was using WSL (Windows Subsystem for Linux), I followed these steps to install Cloudfox:

### First, I installed the necessary dependencies using:
sudo apt update
sudo apt install -y curl unzip

### Then, I downloaded the Cloudfox binary from GitHub:

curl -L https://github.com/cloudfoxable/cloudfox/releases/download/v1.0.0/cloudfox-linux-amd64 -o cl

### After downloading, I made the file executable:

chmod +x cloudfox

### Finally, I moved the binary to a directory in my PATH, like /usr/local/bin/:

sudo mv cloudfox /usr/local/bin/

7. Find Instance ID
Once Cloudfox was installed, I ran this command to list the instances and find the instance ID:

cloudfox aws -p cloudfoxable instances -v2

8. Obtain the SSM Command
From the output, I accessed the loot file, which contained the necessary command to connect to the instance using AWS Systems Manager (SSM).

9. Install Session Manager Plugin
Following AWS's official documentation, I installed the Session Manager plugin, which is needed to connect to the EC2 instance.

10. Connect to the Instance
I then used the command from the loot file to connect to the instance via SSM.

11. Check Permissions
To check the instance's permissions, I ran the following command:
cloudfox aws -p cloudfoxable permissions --principal [NameOfInstanceRole]

This revealed that the instance had access to an S3 bucket.

12. Access the S3 Bucket
I listed the contents of the S3 bucket using the following command:
aws s3 ls s3://<bucket-name>

I looked for the flag.txt file.

13. Retrieve the Flag
Once I found the flag.txt file, I retrieved it using:
aws s3 cp s3://<bucket-name>/file.txt -

I then pasted the flag in the Cloudfoxable portal to complete the challenge.

14. Cleanup Tasks
After finishing the challenge, I switched the bastion_enabled flag back to false in the terraform.tfvars file:
bastion_enabled = false
terraform apply

## Reflection
### What was your approach?
I took a methodical approach to resolve the issue. First, I tackled the Terraform error by debugging the IP issue. I manually added my IP to continue the process. After applying Terraform successfully, I used Cloudfox to find the instance ID, and then I followed the steps to access the EC2 instance and retrieve the flag.

### What was the biggest challenge?
The biggest challenge I faced was the issue with dynamically fetching the public IP in Terraform. Initially, I couldn't understand why the USER_IP variable was returning HTML instead of my public IP. It took a while to debug and figure out that the response from https://ifconfig.me was an HTML page instead of my IP address.

### How did you overcome the challenges?
I solved the problem by manually fetching my public IP using curl ifconfig.me and hardcoding it into the terraform.tfvars file. This bypassed the issue with dynamically fetching the IP and allowed Terraform to apply successfully.

### What led to the breakthrough?
The breakthrough came when I decided to check if the variables were resolving correctly. I traced the issue to the public IP retrieval logic, and once I realized that the response from ifconfig.me was an HTML page instead of my IP, I knew that hardcoding the IP would fix the issue.

### How can the learning be used?
This experience taught me the importance of validating external dependencies when working with cloud infrastructure. Ensuring dynamic inputs, like public IP retrieval, are reliable can prevent issues in automation pipelines. From a defensive perspective, it’s crucial to monitor and validate incoming data from external services to avoid unexpected behavior or vulnerabilities. Regularly reviewing configurations and external interactions can help identify and address potential weaknesses early on.

