# Step - 1 : Create EKS Management Host in AWS #

1) Launch new Ubuntu VM using AWS Ec2 ( t2.micro )	  
2) Connect to machine and install kubectl using below commands  
	$ curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl <br/>
	$ chmod +x ./kubectl <br/>
	$ sudo mv ./kubectl /usr/local/bin <br/>
	$ kubectl version --short --client <br/>

3) Install AWS CLI latest version using below commands 

	$ sudo apt install unzip <br/>
	$ cd  <br/>
	$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" <br/>
	$ unzip awscliv2.zip <br/>
	$ sudo ./aws/install <br/>
	$ aws --version <br/>

4) Install eksctl using below commands <br/>
	$ curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp <br/>
	$ sudo mv /tmp/eksctl /usr/local/bin <br/>
	$ eksctl version <br/>

# Step - 2 : Create IAM role & attach to EKS Management Host #

1) Create New Role using IAM service ( Select Usecase - ec2 ) 	
2) Add below permissions for the role <br/>
	- IAM - fullaccess <br/>
	- VPC - fullaccess <br/>
	- EC2 - fullaccess  <br/>
	- CloudFomration - fullaccess  <br/>
	- Administrator - acces <br/>
		
3) Enter Role Name (eksroleec2) 
4) Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created) 

# Step - 3 : Create EKS Cluster using eksctl # 
**Syntax:** 

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>

**N. Virgina Region: $ eksctl create cluster --name ashokit-cluster4 --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b**
	
**Mumbai Region: $ eksctl create cluster --name ashokit-cluster4 --region ap-south-1 --node-type t2.medium  --zones ap-south-1a,ap-south-1b**	

Note: Cluster creation will take 5 to 10 mins of time (we have to wait). After cluster created we can check nodes using below command.	

$ kubectl get nodes  

# Step - 4 : Update EKS Cluster Config File in EKS Management Host #
	
1) Execute below command in Eks Management host & copy kube config file data <br/>
	$ cat .kube/config 

2) Execute below commands in Jenkins Server and paste kube config file  <br/>
	$ sudo mkdir .kube  <br/>
	$ sudo vi .kube/config  <br/>

3) check eks nodes <br/>
	$ kubectl get nodes 

**Note: We should be able to see EKS cluster nodes here.**

# We are done with our Setup #
	
## Step - 14 : After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing ##