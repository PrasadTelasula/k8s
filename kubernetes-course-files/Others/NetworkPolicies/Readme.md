kubectl apply -f web_deployment.yml

kubectl apply -f app_deployment.yml

kubectl get all -o wide

kubectl run nginx --image=nginx --restart=Never

kubectl exec -it nginx -- bash
    curl appservice.default.svc.cluster.local # You will get response
    curl webservice.default.svc.cluster.local # You will get response
    
kubectl apply app_networkpolicies.yml

kubectl get networkpolicies


kubectl exec -it nginx -- bash
    curl appservice.default.svc.cluster.local # You will not get response
    curl webservice.default.svc.cluster.local # You will get response
    
    
kubectl exec -it <web_pod> -- sh
      apk update
      apk add curl
      curl appservice.default.svc.cluster.local # you will get response
