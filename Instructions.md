## Demo Instructions for minikube

## Requirements 
kubectl 
helm 
minikube 

## minikube installation 

Initially we need to have the following in place on our systems, the instructions found should be viable across x86 architecture and across Windows, Linux and MacOS operating systems. 

- minikube - https://minikube.sigs.k8s.io/docs/start/ 
- helm - https://helm.sh/docs/intro/install/
- kubectl - https://kubernetes.io/docs/tasks/tools/ 

The first time you run the command below you will have to wait for the images to be downloaded locally to your machine, if you remove the container-runtime then the default will use docker. You can also add --driver=virtualbox if you want to use local virtualisation on your system. 

for reference on my ubuntu laptop this process took 6m 52s to deploy the minikube cluster

```
minikube start --addons volumesnapshots,csi-hostpath-driver --apiserver-port=6443 --container-runtime=containerd -p mc-demo --kubernetes-version=1.23.15 
```

## Kasten K10 deployment 
Add the Kasten Helm repository

``` 
helm repo add kasten https://charts.kasten.io/
```
Create the namespace and deploy K10, note that this will take around 5 mins 

```
helm install k10 kasten/k10 --namespace=kasten-io --set auth.tokenAuth.enabled=true --set injectKanisterSidecar.enabled=true --set-string injectKanisterSidecar.namespaceSelector.matchLabels.k10/injectKanisterSidecar=true --create-namespace
```
You can watch the pods come up by running the following command.
```
kubectl get pods -n kasten-io -w
```
port forward to access the K10 dashboard, open a new terminal to run the below command

```
kubectl --namespace kasten-io port-forward service/gateway 8080:8000
```

The Kasten dashboard will be available at: `http://127.0.0.1:8080/k10/#/`

To authenticate with the dashboard we now need the token which we can get with the following command. 

```
TOKEN_NAME=$(kubectl get secret --namespace kasten-io|grep k10-k10-token | cut -d " " -f 1)
TOKEN=$(kubectl get secret --namespace kasten-io $TOKEN_NAME -o jsonpath="{.data.token}" | base64 --decode)

echo "Token value: "
echo $TOKEN
```

## StorageClass Configuration 

Out of the box Kasten K10 should be installed on the standard storageclass within your cluster this ensures that all pods and services are available. If you run this change below beforehand and the CSI storage is used then there is an issue with Prometheus on the deployment (prometheus storage is access denied). 

Annotate the CSI Hostpath VolumeSnapshotClass for use with K10

```
kubectl annotate volumesnapshotclass csi-hostpath-snapclass \
    k10.kasten.io/is-snapshot-class=true
```
we also need to change our default storageclass with the following 

```
kubectl patch storageclass csi-hostpath-sc -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'

kubectl patch storageclass standard -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```
## Deploy K10 Demo App 

Now to deploy the demo application, firstly add the helm repository. 

` helm repo add k10app https://k10app.github.io/k10app`

`helm repo update`

The following command will create and deploy the k10app. 

`helm install k10app -n k10app k10app/k10app --create-namespace`

If you are running on minikube or local kubernetes cluster then you may need to set permissions as per below

`helm install k10app -n k10app k10app/k10app --set mongodb.volumePermissions.enabled=True --set mysql.volumePermissions.enabled=True --set postgresql.volumePermissions.enabled=True --create-namespace` 

When the app and all components are running / completed then you can port forward to use the application. 

`sudo kubectl --kubeconfig /home/michael/.kube/config port-forward --namespace k10app --address 0.0.0.0 service/router 80:80`

At this stage you can open a browser and navigate the application, you can also protect the application and changes using K10. 

If you would like to look within the databases and tables and manipulate that data this can be done in the following sections. 

## Accessing mysql with mysql-workbench (Users)

Port Forward the mysql service for external access

`kubectl port-forward -n k10app service/mysql 3306:3306`

You will need a password to access the instance, use the following command to get this information.

`kubectl get secret --namespace k10app mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode`

## Accessing postgres with PGadmin (Orders) 

Port Forward the Postgres service for external access

`kubectl port-forward service/postgres-postgresql -n k10app 5432:5432`

You will need a password to access the instance, use the following command to get this information.

`kubectl get secret --namespace k10app postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 --decode`

## Accessing MongoDB with Mongo Compass (Catalog)
!! This is not the correct process !! 
`kubectl port-forward service/mongo-mongodb-arbiter-headless -n k10app 27017:27017`

`kubectl get secret --namespace k10app mongo-mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 --decode`

## Blueprints 
!! Work In Progress !! 
annotate for each data service 

## Use cases 
Accidental Mistakes 
Malicious Activity 
Real life transactions 
