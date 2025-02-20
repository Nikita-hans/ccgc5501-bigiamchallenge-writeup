## Reflection on Setting Up My Personal AWS Environment
## Steps I Took
1. Created an AWS Account: I began by setting up a new AWS account to start configuring my cloud setup.

2. Enabled MFA for the Root User: Right away, I activated Multi-Factor Authentication (MFA) for the root user using Microsoft Authenticator. This way, even if someone gets my root credentials, they'd still need my phone to access the account.

3. Created an Administrator User: I created a new user with Administrator Access to avoid using the root account for daily tasks. This follows AWS best practices to keep the account secure.

4. Set Up Admin Password Reset: I enabled the option to reset the admin password at the next login. This ensures I can regain control of my account if there are issues with my access.

5. Enabled MFA for the Admin User: To add more security, I turned on MFA for the administrator user too. This extra step helps protect the account and makes sure that only authorized people can access it.

## Problems I Encountered
* Getting Used to AWS: As I was new to AWS, I found the user interface (UI) a bit overwhelming at first. There were so many options and features, which made it tricky to know where to start or how to navigate through the setup process.

* Assigning Admin Role to a User: I also had some trouble when trying to assign the "Administrator" role to a user. The IAM (Identity and Access Management) interface wasn’t very clear to me, and I wasn’t sure how to give the right permissions.

## How I Solved the Problems
* Consulted AWS Documentation: When I faced difficulties, I turned to the official AWS documentation. This guide gave me clear, step-by-step instructions that helped me assign the admin role to a user and understand the IAM interface better. The documentation was a big help in clearing up confusion and boosted my confidence with AWS.
## Security Steps I Took
* Multi-Factor Authentication (MFA): I set up MFA for both the root and admin accounts. This keeps my account safe by needing both my password and a second factor (my phone) for login.

* Admin User (Not Using Root for Daily Tasks): I created an admin user to follow the "least privilege" rule. By using an admin user for daily tasks, I lower the chances of the root user being compromised.

* Password Reset for Admin: The option to reset the admin password on the next login ensures that I can recover access quickly if needed, which adds an extra layer of security.

## Why Do We Need an Admin User When We Have Root Access?
The root user should only be used for important account-related tasks to reduce security risks.
Using an admin user for everyday tasks follows the principle of least privilege, which helps lessen the damage if the account is hacked.
Since the root user has access to sensitive settings, limiting its use helps protect the overall security of the account.