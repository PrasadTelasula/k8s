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
kubectl apply -f ALBIngressControllerIAMPolicy.json
````


# Step 03
## Create an IAM role using eksctl
#Replaced region, name, cluster and policy arn (Policy arn we took note in step-03)
````bash
eksctl create iamserviceaccount \
    --region ap-south-1 \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster formaceksdemo \
    --attach-policy-arn arn:aws:iam::XXXXXXXXXXXX:policy/ALBIngressControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --approve
 ````
 
 # Verify IAM Account
 ````bash
 # Get IAM Service Account
eksctl  get iamserviceaccount --cluster formaceksdemo
 ````
