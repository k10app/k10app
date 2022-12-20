# k10app

The purpose of this app or the inital goals are: 

- Simple demo of backup and recovery.
- Transform the application on restore.
- Disaster Recovery of App from Cluster A to B.
- Show multiple data services (postgres, mysql, mongodb)
- The ability to show existing Kanister blueprints against these data services.

# Deployment
## Install

Create namespace and add the repository
```
kubectl create ns k10app
helm repo add k10app http://dewin.me/k10app
```

Install it with router as ClusterIP (default)
```
helm install --namespace k10app k10app k10app/k10app
```

Install with router as Loadbalancer
```
helm install --namespace k10app --set serviceType=LoadBalancer k10app k10app/k10app
```

Install with a different storage class
```
helm install --namespace k10app --set global.storageClass=<custom storage class> k10app k10app/k10app
```

## Port mapping
!! Always has to match port 80 !!
```
kubectl port-forward --namespace k10app --address 0.0.0.0 service/router 80:80
```

(OPTIONAL) with an external server
```
kubectl port-forward --namespace k10app --address 0.0.0.0 service/router 1080:80
ssh -L 80:127.0.0.1:1080 <server>
```


