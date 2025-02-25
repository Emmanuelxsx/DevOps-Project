## A microservice architecture deployed on AWS using Terraform(Iac) and Kubernetes

# Creating VPC template Terraform

* VPC config file

![alt text](<images/VPC_with_EC2.tf file.PNG>)
![alt text](<images/VPC_with_EC2.tf file(contd 1).PNG>)
![alt text](<images/VPC_with_EC2.tf file(contd 2).PNG>)

* create a variable file to be passed into the parameter in the VPC config file above;

![alt text](<images/terraform.tfvars file(variables used).PNG>)

* Create an eks.tf file to create EKS cluster on AWS

![alt text](<images/eks/eks.tf terraform code.PNG>)
![alt text](<images/eks/eks.tf terraform code(contd 1).PNG>)
![alt text](<images/eks/eks.tf terraform code(contd 2).PNG>)
![alt text](<images/eks/eks.tf terraform code(contd 3).PNG>)
![alt text](<images/eks/eks.tf terraform code(contd 4).PNG>)

* creating variable for eks cluster

![alt text](<images/eks/variables.tf for eks.tf file.PNG>)

* creating output.tf file for eks cluster
![alt text](<images/eks/output.tf for eks.tf file.PNG>)

* creating output file to display the worker node server IP

![alt text](<images/output.tf file to display the woker node server IP address.PNG>)

* Create security group(sg.tf) file to allow inbound and outbound traffic for eks cluster

![alt text](<images/sg_eks/sg.tf file(security group settings for eks cluster).PNG>)

* creating a security group output file to display the security group created

![alt text](<images/sg_eks/output.tf file to display security group.PNG>)

# Terraform Execution Commands/output

* terraform init: to initialize terraform file

![alt text](<images/terraform commands/terraform init command(to initialize the tf config files).PNG>)

* terraform plan: to view what infrastructure that would be provisioned on AWS

![alt text](<images/terraform commands/terrform plan output(to view what infrastructure that will be provisioned on AWS).PNG>)
![alt text](<images/terraform commands/terrform plan output (contd 1).PNG>)
![alt text](<images/terraform commands/terrform plan output (contd 2).PNG>)

* outputs to be displayed

![alt text](<images/terraform commands/terrform plan output (contd 14).PNG>)

* terraform apply: to provision the infrastructure on AWS

![alt text](<images/terraform commands/terraform apply.PNG>)
![alt text](<images/terraform commands/terraform apply(contd 1).PNG>)
![alt text](<images/terraform commands/terrform apply in action.PNG>)
![alt text](<images/terraform commands/terrform apply in action(contd).PNG>)

# Kubernetes Implementation

* Access the eks cluster provisoned on AWS with Terraform

![alt text](<images/terraform commands/eks cluster created on AWS.PNG>)

* connecting to the eks cluster created on AWS

![alt text](<images/terraform commands/adding eks cluster.PNG>)

**Note:** An online boutique microservice application docker images is stored in a docker hub repository as shown below;

![alt text](<images/terraform commands/docker images.png>)

# Creating a Microservice Architecture Kubernetes Configuration of an online boutique application

![alt text](<images/terraform commands/config file/1.PNG>)
![alt text](<images/terraform commands/config file/2.PNG>)
![alt text](<images/terraform commands/config file/3.PNG>)
![alt text](<images/terraform commands/config file/4.PNG>)
![alt text](<images/terraform commands/config file/5.PNG>)
![alt text](<images/terraform commands/config file/6.PNG>)
![alt text](<images/terraform commands/config file/7.PNG>)
![alt text](<images/terraform commands/config file/8.PNG>)
![alt text](<images/terraform commands/config file/9.PNG>)
![alt text](<images/terraform commands/config file/10.PNG>)
![alt text](<images/terraform commands/config file/11.PNG>)
![alt text](<images/terraform commands/config file/12.PNG>)
![alt text](<images/terraform commands/config file/13.PNG>)
![alt text](<images/terraform commands/config file/14.PNG>)
![alt text](<images/terraform commands/config file/15.PNG>)

# Next install kubectl to interact with the EKS cluster

![alt text](<images/terraform commands/installing kubectl to interact with eks cluster.PNG>)

* checking the kubectl version

![alt text](<images/terraform commands/kubectl version.PNG>)

* Applying the microservice configuration file into the EKS cluster

![alt text](<images/terraform commands/applying the microservice archiecture config file in the eks cluster.PNG>)

* viewing the pods and the woker nodes managing each microservice application

![alt text](<images/terraform commands/viewing the pods and the woker nodes managing each microservice application.PNG>)

* Checking the service to access the application on browser through the frontend load balancer external address

![alt text](<images/terraform commands/viewing the kubernetes service.PNG>)

## THE MICROSERVICE ONLINE BOUTIQUE SHOPPING APPLICATION WITH A CARD FRAUD DETECTION SYSTEM ON BROWSER 

* Front-page after deploying on AWS

![alt text](<images/terraform commands/frontend/implem/FRONTEND1.jpg>)

* selecting a product

![alt text](<images/terraform commands/frontend/implem/2ND PAGE SELECTING PRODUCT.jpg>)

* The Cartservice

![alt text](<images/terraform commands/frontend/implem/3RD PAGE CARTSERVICE.jpg>)

* the checkout/payment page

![alt text](<images/terraform commands/frontend/implem/4TH PAGE.jpg>)


# THE FRAUD DETECTION

* the system if camera not enabled

![alt text](<images/terraform commands/frontend/implem/IF CAMERA NOT ENABLED.PNG>)

* When camera is enabled and user enters a flagged card

![alt text](<images/terraform commands/frontend/implem/when camera enabled.jpg>)

* An order after using a flagged card

 ![alt text](<images/terraform commands/frontend/implem/using a flagged card.jpg>)

 * An order using with an authentic card

![alt text](<images/terraform commands/frontend/using authentic card.png>)

* Fraud alert

![alt text](<images/terraform commands/frontend/implem/fraud alert.png>)

* location through IP address 

![alt text](<images/terraform commands/frontend/implem/location through IP addr.png>)

* Submission page

![alt text](<images/terraform commands/frontend/implem/SUBMISSION PAGE 5TH.PNG>)