Step 01: Create Service Account 
# List Service Accounts
kubectl get sa -n kube-system

# Create ClusterRole, ClusterRoleBinding & ServiceAccount
kubectl apply -f rbac-role.yaml

# List Service Accounts
kubectl get sa -n kube-system

# Describe Service Account alb-ingress-controller 
kubectl describe sa alb-ingress-controller -n kube-system

Step 02: 
# Replaced region, name, cluster and policy arn (Policy arn we took note in step-03)
eksctl create iamserviceaccount \
    --region ap-south-1 \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster formaceksdemo \
    --attach-policy-arn arn:aws:iam::304370290957:policy/ALBIngressControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --approve
