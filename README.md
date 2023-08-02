# K10app Demo Application

<p align="center">
 <img src="K10_StealthApp.png?raw=true" alt="K10App Logo" width="20%" height="20%" />
</p>

The purpose of this app or the initial goals are: 

- Simple demo of backup and recovery.
- Transform the application on restore.
- Disaster Recovery of App from Cluster A to B.
- Show multiple data services (postgres, MySQL, mongodb)
- The ability to show Kanister blueprints against these data services.

# Architecture 

![](K10_app-App%20Logic.png)

The K10App is a demonstration e-commerce site. 

The K10App is a cloud-first microservices demo application. The K10App is a microservices application. The application is a web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

Kasten uses this application to demonstrate the use of Data Protection use cases like Data services backup & recovery, Disaster Recovery and Application Consistency. This application works on a Kubernetes cluster. 

It’s easy to deploy with little to no configuration.

If you’re using this demo, please ★Star this repository to show your interest!
## Deployment

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

(OPTIONAL) if you want to route it over an SSH tunnel, use an in-between port (e.g. 1080)

```shell
kubectl port-forward --namespace k10app --address 0.0.0.0 service/router 1080:80
ssh -L 80:127.0.0.1:1080 <server>
```

## Deploy Kasten K10

[Start Here](https://docs.kasten.io/latest/install/requirements.html)
## Application Consistency 

Whilst you can protect your K10App in a crash-consistent state with Kasten K10, we should consider consistency when it comes to protecting our "Mission-Critical" workloads. For this we have created blueprints. 

***We need to be clear here that these blueprints are for demo purposes and shows the power of Kanister they are not strictly supported by Kasten support and the supported blueprints can be found in the docs.*** 

We have 3 different data services that make up our application. We must provide a way to consistently protect these when high loads of transactions are hitting our databases. 

Blueprints can be found in the /blueprints folder. 

### MySQL 

Like all good online shops, we need a way to login and authenticate with the site. All of our user data is stored within this MySQL data service. 

This is an example command pulling directly from github, we would need to change to the correct URL 

`kubectl --namespace kasten-io apply -f https://raw.githubusercontent.com/k10app/k10app/main/blueprints/k10app-mysql-blueprint.yml`

We then need to annotate our mysql statefulset 

` kubectl --namespace k10app annotate statefulset/mysql kanister.kasten.io/blueprint=k10app-mysql-blueprint`

### PostgreSQL

PostgreSQL is used to store our order information, firstly we need to add our blueprint to the kasten-io namespace. 

`kubectl --namespace kasten-io apply -f https://raw.githubusercontent.com/k10app/k10app/main/blueprints/k10app-postgres-blueprint.yml`

following this we can now annotate the statefulset.

` kubectl --namespace k10app annotate statefulset/postgres-postgresql kanister.kasten.io/blueprint=k10app-postgres-blueprint`

### MongoDB

Our final data service consists of our precious goods, the catalog database enables us to see our stock levels. This also has a hidden attack console to simulate manipulation with our stock levels and products available. We first add our blueprint. 

`kubectl --namespace kasten-io apply -f https://raw.githubusercontent.com/k10app/k10app/main/blueprints/k10app-mongodb-blueprint.yml`

Then we annotate the mongo statefulset. 

` kubectl --namespace k10app annotate statefulset/mongo-mongodb kanister.kasten.io/blueprint=k10app-mongodb-blueprint`

## Operations - Demos

WIP 

- Easiest Demo is manipulating the stock levels using the unsecured admin portal. (Change Stock Levels, Change Products, Remove Products)
- You could port-forward the databases and connect to be malicuous (Delete Users, Delete Orders, Change Orders)
- A stretch goal would be to simulate transactions and show the power of consistency within the application. 
- Clone Scenario, Imagine your dev team would like a copy of the dataset for your user database, this contains potentially sensitive data. How would you mask that data but stil provide the dev with a copy of good data. 
- Disaster Recovery, How could we make sure we are protected against bad things and have a copy ready and waiting to run in a secondary location. 
- Application Mobility, a demonstration of being able to move and manipulate the application by transforming areas of the application. 


