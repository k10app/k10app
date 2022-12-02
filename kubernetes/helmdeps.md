```
NSK10=k10app

helm repo add bitnami https://charts.bitnami.com/bitnami
helm install --namespace $NSK10 mongo bitnami/mongodb  --set architecture="replicaset"
helm install --namespace $NSK10 mysql bitnami/mysql --set auth.database="userdb"


```





# Mongo output
To get the root password run:

    export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace k10app mongo-mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 -d)

To connect to your database, create a MongoDB&reg; client container:

    kubectl run --namespace k10app mongo-mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:6.0.3-debian-11-r0 --command -- bash

Then, run the following command:

    mongosh admin --host "mongo-mongodb-0.mongo-mongodb-headless.k10app.svc.cluster.local:27017,mongo-mongodb-1.mongo-mongodb-headless.k10app.svc.cluster.local:27017" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD

# MySQL output

Services:

  echo Primary: mysql.k10app.svc.cluster.local:3306

Execute the following to get the administrator credentials:

  echo Username: root
  MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace k10app mysql -o jsonpath="{.data.mysql-root-password}" | base64 -d)

To connect to your database:

  1. Run a pod that you can use as a client:

      kubectl run mysql-client --rm --tty -i --restart='Never' --image  docker.io/bitnami/mysql:8.0.31-debian-11-r10 --namespace k10app --env MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD --command -- bash

  2. To connect to primary service (read/write):

      mysql -h mysql.k10app.svc.cluster.local -uroot -p"$MYSQL_ROOT_PASSWORD"