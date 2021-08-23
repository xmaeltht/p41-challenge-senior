# P41-DevOps-Challenge  -   Senior Level


This is a senior-level challenge for candidates who want to join the Particle41 DevOps team.

It is designed to test your level of familiarity with development and operations tools and concepts.

You will have **8 hours** to complete the challenge.


## The challenge

You must use either Terraform, AWS CloudFormation, AWS CDK, or Pulumi for all of the following tasks.

- Create code for deploying a VPC in AWS with 2 public and 2 private subnets.

- Create code for deploying an EKS cluster in AWS, which will use the VPC created in the previous step. The cluster must have 2 nodes, using instance type `t3a.large`. The nodes must be on the private subnets only.

- Add a `README.md` to the root directory of your project, with instructions for the team to deploy the infrastructure you created.

- Publish your code to a public Git repository in a platform of your choice (e.g. GitHub, GitLab, Bitbucket, etc.), so that it can be cloned by the team.


## Criteria


Your task will be considered successful if a colleague is able to deploy your infrastructure to an AWS account.

Imagine that someone with less experience than you will need to clone your repository and deploy your infrastructure. With that in mind, you must provide all the instructions they will need to do that successfully. These must include any prerequisites for deployment; mention of needed tools and links to their installation pages; how to configure credentials for the tool of your choice; and what commands to run for deploying your code to AWS.

We want to see your ability to properly document and communicate about your work with the team.

Other criteria for evaluation will be:

- Code quality and style: your code must be easy for others to read, and properly documented when relevant


# CHALLENGE SOLUTION

## Prerequisite 

##### Installation of dependencies

There is a few dependencies that need to be install in order to complet this challenge:

NOTE: This installation may vary accoording to your system

    - Git
    - terraform
    - tfenv or tfswitch
    - awscli
    - kubectl

##### Setup of dependencies

There is a few dependencies that need to be setup in order to complet this challenge:

* tfenv or tfswitch need to be set to an older version of terraform **v0.13.7** by running:

If using tfenv:
```shell
    $ tfenv install 0.13.7 
    $ tfenv use 0.13.7
```
If using tfswitch:
```shell
    $ tfswitch ( use the up and down arrow to choose the appropiate version )
```
* AWS CLI

[Configure AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
Make sure your user has **"Admin"** privileges but is not **"root"** (Best Practices) - Ask your AWS Administrator to create a user for you and send you access keys to be able to use AWS CLI. 

```sh
aws configure

    AWS Access Key ID [None]: <Your AWS Access Key ID - Ask your administrator>
    AWS Secret Access Key [None]: <Your AWS Secret Access Key - Ask your administrator>
    Default region name [None]: <Your prefer region>
    Default output format [None]: json
```

Once done with setup the next step is to acctually the running process of the code.
Here are the step for it:

Open terminal anf run these following command: 

    git clone https://gitlab.com/xmaeltht/p41-senior-challenge.git

    cd p41-senior-challenge

Terraform Initialize and Downloading all the necessary providers
    
    terraform init

Terraform plan which shows all the infrastructure that will be deployed
    
    terraform plan 

Terraform apply which will deploy all the necessary infrastructure on AWS
Once all the deployable resources look correct in the plan 

    terraform apply 

#### **NOTE** 
This deployment might take some time. Please be **patient**.
At the end of this deployment a config file will be generate in your current directory in ./p41-eks-config you can add it to your default ~/.kube if you want to. otherwise you will need to provide the path of your config file when running the kubectl command
**Example:**    

    kubectl get node --kubeconfig ./p41-eks-config

**output** 
```
NAME                                       STATUS   ROLES    AGE     VERSION
ip-10-0-1-86.us-east-2.compute.internal    Ready    <none>   2m52s   v1.21.2-eks-c1718fb
ip-10-0-2-112.us-east-2.compute.internal   Ready    <none>   2m47s   v1.21.2-eks-c1718fb
```
If you decide to add the p41-eks-config into your default kubernetes config file you will run these command:

    $ cat p41-eks-config >> ~/.kube/config
To check if everything looks good run

    $ kubectl get node 


## Inputs

| Name | Description | Required | Type | Default Value |
|------|-------------|----------|------|---------------|
| region | AWS Deployment Region | string | no | "us-east-2" |
| cluster_name | AWS Cluster Name | string | no | "p41_eks_cluster" |
| vpc_name | AWS VPC Name | string | no | "p41-vpc-eks" |
| eksnode_instance_type | EKS Node Instance Type | string | no | "t3a.large" |
| vpc_cidr | VPC CIDR Block  | string | no | "10.0.0.0/16" |
| private_subnets | VPC Private Subnets | list | no | ["10.0.1.0/24", "10.0.2.0/24"] |
| public_subnets | VPC Private Subnets | list | no | ["10.0.100.0/24", "10.0.101.0/24"] |



## Outputs

| Name | Description |
|------|-------------|
| cluster_endpoint | Endpoint for EKS control plane. |
| p41-eks-config| Kubernetes Config on Nodes |
| region | AWS Deployment Region |


## WARNING ##

Make sure to cleanup all your infrastructure once testing is done since these resources are not eligible to be used with the **FREE** AWS Tier. 

```shell
terraform destroy # You will have to enter 'yes' in the input for final confirmation 
# If you aknow what you are doing you can flag the command with  -auto-approve
```