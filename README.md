# Docupet - My Furry's Friend

**Automated Deployment Pipeline for Pet Licensing Web App**

**Overview**

This repository contains the source code and infrastructure configuration for a Pet Licensing Web App. The application is built using Java and relies on the LAMP (Linux, Apache, MySQL, PHP) stack. The Pet Licensing Web App is deployed using an automated pipeline with Jenkins. The pipeline incorporates Terraform for infrastructure provisioning and Ansible for configuration. The Jenkinsfile provided orchestrates the entire deployment process, ensuring a seamless and hands-free deployment.


**1- Directory structure**


![image](https://github.com/lymdocupet/my-furry-friend/assets/157446700/fdde7d7c-8be0-44a6-bd12-30cb82facb04)



**2- Terraform Configuration**

    The infrastructure is defined using Terraform, with the main configuration in the 1-pet-infra/terraform directory. Key components include:
        - The Terraform backend is configured to store the state file in an S3 bucket.
        - main.tf: Defines the main infrastructure components such as VPC, EC2 instances, RDS, etc.
        - modules/: Contains Terraform modules for different components like EC2, RDS, security groups, and VPC.
        - terraform.tfvars: Variable values used in the Terraform configuration.
        - ansible_vars.json: Generated file containing dynamic values for Ansible configuration.

**3- Ansible Configuration**

Ansible is used for configuration management and is located in the 1-pet-infra/ansible directory. Key components include:

        - site.yml: Main Ansible playbook.
        - roles/: Contains Ansible roles for different tasks, such as configuring Apache, Git, MySQL, and PHP.

Feel free to customize the configuration files and code according to your specific requirements.

**4- Jenkins Pipeline**

The CI/CD pipeline is defined in the Jenkinsfile. It includes stages for Terraform apply/plan/destroy and Ansible configuration. Parameters for approval are defined in the Jenkins job.

Prerequisites
Before setting up the Jenkins pipeline, ensure the following prerequisites are in place:

a. Jenkins installed and configured with usual recommended plugins as well as terraform, ansible, git and aws plugin. Ensure you have python3.
b. AWS credentials stored in Jenkins Credentials with the ID 'AWS_cred'.

Create a new Jenkins job and configure it to use the provided Jenkinsfile.

When triggering the job, specify the desired parameters:
 
    CREATE_RESOURCES: Create Terraform resources (Default: false)
    DESTROY_RESOURCES: Destroy Terraform resources (Default: false)
    ASK_FOR_CONFIRMATION: Ask for approval before applying or destroying (Default: true)
    ONLY_PLAN: Run Terraform plan only (Default: false)

**5- Jenkins Pipeline Steps**

Terraform Apply/Plan/Destroy
Initialization: Terraform initialization in the specified directory.

Plan Execution:

Display Terraform plan.
If ONLY_PLAN is true, only display the plan and skip further execution.

Approval Prompt:
If ASK_FOR_CONFIRMATION is selected and either CREATE_RESOURCES or DESTROY_RESOURCES is true:
If CREATE_RESOURCES is true, prompt for approval to apply Terraform changes.
If DESTROY_RESOURCES is true, prompt for approval to destroy the infrastructure.
If no approval required, proceed without prompting.

Apply/Destroy Execution:

If CREATE_RESOURCES is true, apply Terraform changes without manual approval.
If DESTROY_RESOURCES is true, destroy infrastructure without manual approval.

Output Display:
Display the generated RDS endpoint and web server IP address for future reference.

Ansible Configuration
Ansible Execution:
Change to the Ansible directory.
Run Ansible playbook only if either CREATE_RESOURCES or DESTROY_RESOURCES is selected.

**6- Post-Deployment Actions**

Display success or failure messages based on the deployment outcome.
Obtaining Deployment Information
After the pipeline execution, the following information will be displayed in the Jenkins console output:

RDS Endpoint: The hostname for the deployed RDS instance.
Web Server IP Address: The IP address of the deployed web server.
Use this information for accessing the Pet Licensing Web App and establishing database connections.

**7- Test the Application**

Open your web browser and go to http://<web-server-ip>.
Verify the database connection by performing the following actions:
a. Register a new account.
b. Log in with the newly created account.
c. Book a pet licensing plan.

Additionally, you have the option to access your pet licensing web server via SSH and examine the database using MySQL. Use the following command:

**ssh -i ~/.ssh/docupet.pem ubuntu@<web-server-ip-address>**

Once connected, log in to MySQL with the password "Password0123":

**mysql -u admin -p -h <rds-endpoint/hostname>**

Within the MySQL terminal, execute commands to explore databases, tables, and data:

- to display a list of databases
  
    show databases;
  
- to switch to docupet database
  
    use docupetdb;
  
- to shows a list of tables
  
    show tables;
  
- to view the registered users
  
    select * from tbluser;
  
- to view the booked plans
  
    select * from tblbooking;

**Conclusion:**

You have successfully set up an automated deployment pipeline with Jenkins for your Pet Licensing Web App. Adjust the job parameters as needed for different deployment scenarios. Happy automated deploying!
