# k10app

The purpose of this app or the inital goals are: 

- Simple demo of backup and recovery.
- Transform the application on restore.
- Disaster Recovery of App from Cluster A to B.
- Show multiple data services (postgres, mysql, mongodb)
- The ability to show existing Kanister blueprints against these data services.

# Deployment

## Install

Create the namespace to install the chart to

```shell
kubectl create ns k10app
```

Install it with router as ClusterIP (default)

```shell
helm install --namespace k10app k10app oci://ghcr.io/k10app/charts/k10app --version 0.1.0
```

Install with router as Loadbalancer

```shell
helm install --namespace k10app --set serviceType=LoadBalancer k10app oci://ghcr.io/k10app/charts/k10app --version 0.1.0
```

Install with a different storage class

```shell
helm install --namespace k10app --set global.storageClass=<custom storage class> k10app oci://ghcr.io/k10app/charts/k10app --version 0.1.0
```

## Port mapping

The frontend expects the router to answer on port 80. If you are doing port forwarding, you can do so on port 80 to port 80

```shell
kubectl port-forward --namespace k10app --address 0.0.0.0 service/router 80:80
```

(OPTIONAL) if you want to route it over an SSH tunnel, use an inbetween port (eg 1080)

```shell
kubectl port-forward --namespace k10app --address 0.0.0.0 service/router 1080:80
ssh -L 80:127.0.0.1:1080 <server>
```
