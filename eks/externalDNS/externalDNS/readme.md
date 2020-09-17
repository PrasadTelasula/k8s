# Step-01 
## Create IAM Policy

| :point_up:    | Go to AWS Console and create policy with AllowExternalDNSUpdates.json file |
|---------------|:---------------------------------------------------------------------------|

| :exclamation:  Make a note of Policy ARN which we will use in next step   |
|---------------------------------------------------------------------------|

````
arn:aws:iam::XXXXXXXXXXX:policy/AllowExternalDNSUpdates
````

# Step-02
## Create IAM Role, k8s Service Account & Associate IAM Policy
````bash
# Template
eksctl create iamserviceaccount \
    --name service_account_name \
    --namespace service_account_namespace \
    --cluster cluster_name \
    --attach-policy-arn IAM_policy_ARN \
    --approve \
    --override-existing-serviceaccounts

# Replaced name, namespace, cluster, arn 
# Replaced name, namespace, cluster, arn 
eksctl create iamserviceaccount \
    --name external-dns \
    --region ap-south-1 \
    --namespace default \
    --cluster formaceksdemo \
    --attach-policy-arn arn:aws:iam::XXXXXXXXXXX:policy/AllowExternalDNSUpdates \
    --approve \
    --override-existing-serviceaccounts
````

| :memo:        | Verify the Service Account |
|---------------|:------------------------|

````bash
kubectl get sa external-dns
````

| :memo:        | Verify IAM Role & IAM Policy |
|---------------|:------------------------|
- With above step in CFN, we will be landed in IAM Role created for external-dns.
- Verify in Permissions tab we have a policy named AllowExternalDNSUpdates
- Now make a note of that Role ARN, this we need to update in External-DNS k8s manifest
````bash
arn:aws:iam::xxxxxxxxxx:role/eksctl-formaceksdemo-addon-iamserviceaccount-defa-Role1-123H9ZMUEG2B5
````

# Step-03
## Update External DNS Kubernetes manifest
| :zap:        update the external-dns.yml file with the IAM role ARN!   |
|-----------------------------------------|

# Step-04
## Deploy ExternalDNS
````bash
# Deploy external DNS
kubectl apply -f kube-manifests/

# Verify Deployment by checking logs
kubectl logs -f $(kubectl get po | egrep -o 'external-dns[A-Za-z0-9-]+')

# List pods (external-dns pod should be in running state)
kubectl get pods
````
