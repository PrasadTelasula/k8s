```bash
kubectl apply -f web_deployment.yml
````

````bash
kubectl apply -f app_deployment.yml
````

````bash
kubectl get all -o wide
````

````bash
kubectl run nginx --image=nginx --restart=Never
````

````bash 
kubectl exec -it nginx -- bash

     # You will get response
     curl appservice.default.svc.cluster.local
     # You will get response
    `curl webservice.default.svc.cluster.local` 
    
````
    
````bash
kubectl apply app_networkpolicies.yml
````

````bash
kubectl get networkpolicies
````


````bash
kubectl exec -it nginx -- bash
    # You will not get response
    curl appservice.default.svc.cluster.local
    
    # You will get response
    curl webservice.default.svc.cluster.local
````
    
````bash
kubectl exec -it <web_pod> -- sh
      apk update
      apk add curl
      
      # you will get response
      curl appservice.default.svc.cluster.local
````
