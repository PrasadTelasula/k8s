# Step 01: 
## Create Service Account
````bash
#List Service Accounts
kubectl get sa -n kube-system

#Create ClusterRole, ClusterRoleBinding & ServiceAccount
kubectl apply -f rbac-role.yaml

#List Service Accounts
kubectl get sa -n kube-system

#Describe Service Account alb-ingress-controller 
kubectl describe sa alb-ingress-controller -n kube-system
````

# Step 02: 
## Create IAM Policy for ALB Ingress Controller
````bash
Go to Services -> IAM -> Policies -> Create Policy
Click on JSON tab and paste the content from ALBIngressControllerIAMPolicy.json
Come back to Visual Editor
Add ELB full access
Click on Add Additional Permissions
Service: ELB
Actions: All ELB actions (elasticloadbalancing:*)
Resources: All Resources
Remove ELB which has warnings
Click on Remove
Click on Review Policy
Name: ALBIngressControllerIAMPolicy
Description: This IAM policy will allow our ALB Ingress Controller pod to make calls to AWS APIs
Click on Create Policy
````

#Replaced region, name, cluster and policy arn (Policy arn we took note in step-03)
eksctl create iamserviceaccount \
    --region ap-south-1 \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster formaceksdemo \
    --attach-policy-arn arn:aws:iam::304370290957:policy/ALBIngressControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --approve
