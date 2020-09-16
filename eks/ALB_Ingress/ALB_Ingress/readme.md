Step 01: 
# Replaced region, name, cluster and policy arn (Policy arn we took note in step-03)
eksctl create iamserviceaccount \
    --region ap-south-1 \
    --name alb-ingress-controller \
    --namespace kube-system \
    --cluster formaceksdemo \
    --attach-policy-arn arn:aws:iam::304370290957:policy/ALBIngressControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --approve
