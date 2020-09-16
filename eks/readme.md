############################
Step-01: Create EKS Cluster using eksctl
# Create Cluster
eksctl create cluster --name=formaceksdemo \
                      --region=ap-south-1 \
                      --zones=ap-south-1a,ap-south-1b \
                      --without-nodegroup 
                      
# Get List of clusters
eksctl get clusters



Step-02: Create & Associate IAM OIDC Provider for our EKS Cluster
To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create & associate OIDC identity provider.
To do so using eksctl we can use the below command.
Use latest eksctl version (as on today the latest version is 0.21.0)

# Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region ap-south-1 \
    --cluster formaceksdemo \
    --approve
    
Step-03: Create EC2 Keypair
Create a new EC2 Keypair with name as kube-demo
This keypair we will use it when creating the EKS NodeGroup.
This will help us to login to the EKS Worker Nodes using Terminal.

Step-04: Create Node Group with additional Add-Ons in Public Subnets

These add-ons will create the respective IAM policies for us automatically within our Node Group role.

# Create Public Node Group   
eksctl create nodegroup --cluster=formaceksdemo \
                       --region=ap-south-1 \
                       --name=formaceksdemo-ng \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=PublicLaunchKey \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access
