# Run temp container in the ns
```
kubectl run --rm -it --namespace k10app --image alpine bbox -- /bin/sh
```

## Add package
```
apk add curl jq
```
## Test BUS
```
curl http://basicuserservice/user/users
curl http://basicuserservice/user/register -X POST -H 'Content-Type: application/json' -v -d '{"login":"timothy","password":"supersecure","email":"myemail@gmail.com"}'
curl http://basicuserservice/user/login -X POST -H 'Content-Type: application/json' -v -d '{"login":"timothy","password":"supersecure"}'

JWT=$(curl http://basicuserservice/user/login -X POST -H 'Content-Type: application/json' -v -d '{"login":"timothy","password":"supersecure"}' | jq -r .jwt)
JWTAUTH="authorization: Bearer $JWT"
curl -H "$JWTAUTH" -X GET http://basicuserservice/user/verify 
```

## Test Catalog
```
curl  -X GET http://catalog/catalog/list
curl  -X GET http://catalog/catalog/list/638a23bd2f972ece441c78e9
```

## Test Order
```
curl -X GET http://order/order/basket/list
curl -H "$JWTAUTH" -X GET http://order/order/basket/list
curl -H "$JWTAUTH" -X POST -H 'Content-Type: application/json' http://order/order/basket/add -v -d '{"quantity": 1,"catalogId": "638a23bd2f972ece441c78e9"}'
curl -H "$JWTAUTH" -X GET http://order/order/basket/list

curl -H "$JWTAUTH" -X POST -H 'Content-Type: application/json' http://order/order/main/create -d '{}'
curl -H "$JWTAUTH" -X GET http://order/order/main/list

ORDERID=$(curl -H "$JWTAUTH" -X GET http://order/order/main/list | jq .[0].id)
curl -H "$JWTAUTH" -X GET "http://order/order/main/list/$ORDERID"

curl -H "$JWTAUTH" -X POST -H 'Content-Type: application/json' "http://order/order/main/pay/$ORDERID" -d '{"K1SA": "1111 2222 3333 4444","CVC": "111"}'

curl -H "$JWTAUTH" -X GET "http://order/order/main/list/"
```