# Prerequisites
1. Prepare bootstrap instance eks-bootstrap
2. Attach Administrator IAM role.
3. Install 
    - awscli
    - kubectl
              <table>
              <thead>
                <tr>
                  <td align="left">
                    :information_source: Installation
                  </td>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>
                    <ul>
                      <li>curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/amd64/kubectl</li>
                      <li>chmod +x ./kubectl</li>
                      <li>sudo mv ./kubectl /usr/local/bin</li>
                      <li>mkdir -p $HOME/bin && export PATH=$PATH:$HOME/bin</li>
                      <li>echo 'export PATH=$PATH:$HOME/bin' >> ~/.bash_profile</li>
                      <li>kubectl version --short --client</li>
                    </ul>
                  </td>
                </tr>
              </tbody>
            </table>
      

    - eksctl
            <table>
              <thead>
                <tr>
                  <td align="left">
                    :information_source: Installation
                  </td>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>
                    <ul>
                      <li>curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp</li>
                      <li>sudo mv /tmp/eksctl /usr/local/bin</li>
                      <li>eksctl version</li>
                    </ul>
                  </td>
                </tr>
              </tbody>
            </table>
    
       
       
       
     


# Step-01: Create EKS Cluster using eksctl

````bash
## Create Cluster
eksctl create cluster --name=formaceksdemo \
                      --region=ap-south-1 \
                      --zones=ap-south-1a,ap-south-1b \
                      --without-nodegroup 
                      
# Get List of clusters
eksctl get clusters
````


# Step-02: Create & Associate IAM OIDC Provider for our EKS Cluster
1. To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create & associate OIDC identity provider.
2. To do so using eksctl we can use the below command.
3. Use latest eksctl version (as on today the latest version is 0.21.0)

````bash
## Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region ap-south-1 \
    --cluster formaceksdemo \
    --approve
````
    
# Step-03: Create EC2 Keypair
````bash
-> Create a keypair from AWS console.
````

# Step-04: Create Node Group with additional Add-Ons in Public Subnets

## These add-ons will create the respective IAM policies for us automatically within our Node Group role.

````bash
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
 ````
# Add Multiple node groups to the cluster
## Create Linux Node Group 
````bash
eksctl create nodegroup --cluster=formaceksdemo \
                       --region ap-south-1 \
                       --name formaceksdemo-ng-t3 \
                       --node-type t3.medium \
                       --nodes 2 \
                       --nodes-min 2 \
                       --nodes-max 4 \
                       --node-volume-size 20 \
                       --ssh-access \
                       --ssh-public-key PublicLaunchKey \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 
````

# Create Winodws Node group
````bash
eksctl create nodegroup --cluster formaceksdemo \
                        --region ap-south-1 \
                        --name 
                        --node-type t2.large \
                        --nodes 1 \
                        --nodes-min 1 \
                        --nodes-max 2 \
                        --node-ami-family WindowsServer2019FullContainer \
                        --node-volume-size 100 \
                        --ssh-access \
                        --ssh-public-key PublicLaunchKey \
                        --asg-access  \
                        --external-dns-access  \
                        --full-ecr-access  \
                        --appmesh-access  \
                        --alb-ingress-access
                        
# To work with windows Nodes VPC controller utilities should be installed
eksctl utils install-vpc-controllers --name=formaceksdemo --region ap-south-1 --approve
````


# Select Nodes by labels
````bash
# List nodes with labels
kubectl get nodes --show-labels

# List nodes with instance type c5.large
kubectl get nodes -l=node.kubernetes.io/instance-type=c5.xlarge

# List nodes with instance type t3.medium
kubectl get nodes -l=node.kubernetes.io/instance-type=t3.medium

# List nodes with instance type t2.large
kubectl get nodes -l=node.kubernetes.io/instance-type=t2.large

# List all Linux nodes.
kubectl get nodes -l=kubernetes.io/os=linux

# List all windows nodes.
kubectl get nodes -l=kubernetes.io/os=windows
````
