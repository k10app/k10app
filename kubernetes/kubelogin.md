```
docker login -u [yourlogin] ghcr.io

NSK10=k10app
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=/home/[yourlogin]/.docker/config.json \
    --namespace=$NSK10 \
    --type=kubernetes.io/dockerconfigjson


```

