# Read Me


## Cleaning 

We first want to remove old deployments
```
kubectl get deployments
kube delete deployment client-deployment

kubectl get services
kubectl delete service client-node-port
```

## Apply

```
#We could use : 
kubectl apply -f k8s/client-deployment.yaml 

# But there is  a shortcut that will apply a group of config:
kubectl apply -f k8s
```
 