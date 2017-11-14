#tuhin-flask-wave

Introduction
____________

This python flask App deployment in AWS consists of the following components : 
- a bash script to control and execute the deployment - aws-app.sh 
- aws cli code / commands embedded in the bash to build the AWS components required for this deployment (explained later)
- Ansible playbook to handle the configuration management and App deployment post Infrastructure build
- Docker to create the python-flask environment
- Operating system is Ubuntu : it has been choosen due to its ability to handle issues around SELinux bit and 
  host-system firewall Vs Redhat / CentOS . 

Deployment Methodology
______________________

Assumptions :

- The system that you would choose for the deployment of the code must have awscli and Ansible installed and properly configured
- Use bash terminal shell configured with a public network
- I have already tested it in AWS , so if you are using the same account for execution , please delete the created resources -
  like EC2,Security Group and Key-pair before execution

Flow : 

- Copy the script - aws-app.sh in the system . Give execute permission (if required)
- The script when executed :
   1. Creates a base directory for the code and pulls the code repository from github
   2. Creates aws minimum infrastructure needed for the code - EC2 instance , key-pair and security group .The default VPC has 
      been used here
   3. Triggers Ansible 
   4. Ansible takes on the basic configuration management tasks and builds a docker image from Dockerfile
   
Advantages :

- This is a quick way of testing an App on AWS , with focus on App functionality 
- Dockerized environment , keeps the OS pristine
- Code change and re-deployment is fast - just change the app.py code and update Dockerfile 

Limitations:

- The AWS infrastructure used here is not robust and lacks HA and UPTIME best practices
- awscli is used here. For a deployment of a larger scale terraform or cloud formation stacks would be considered

 
 Other CI/CD pipelines could be used to carry out similar deployments (possibly large scale) : 
 
 - Terraform with Ansible and Jenkins Build server using pipeline (use Jenkinsfile to define terraform code with state 
   control measures ), Repo server either Github or Bitbucket 
 - Cloud Formation stack in Bamboo / Jenkins with post-deploy ansible or helper scripts
 - Elastic beanstalk
 - Entire deployment via Ansible / Ansible Tower or Puppet 
 

