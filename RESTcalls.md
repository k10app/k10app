# User service
MariaDB


## /user/register
POST
```
{
    "login":"ed",
    "email":"ed@x.be",
    "password":"secretpass"
}
```

returns

```
{
    "status":"ok",
    "jwt":$jwt
}
```


## /user/login

POST
```
{
    "login":"ed",
    "password":"secretpass"
}
```

returns

```
{
    "status":"ok",
    "jwt":$jwt
}
```

# Catalog service
MongoDB

## /catalog/list

GET

returns
```
[
{
    "id":1,
    "name":"ball",
    "description":"it is a ball",
    "price":3,
    "imgurl":"https://",
    "stock":100
}
]
```

## /catalog/list/[id] (call back from order)
GET 
returns

```
{
    "id":1,
    "name":"ball",
    "description":"it is a ball",
    "price":3,
    "imgurl":"https://",
    "stock":100
}
```

## /catalog/bulkStockUpdate (call from order svc)
***security issue***

POST
```
[
    {
        "id":1,
        "inc":1
    }
]
```

returns
200 
```
{
    "status":"success"
}
```
or
404 : error

# Order service (basket service)
PostgresSQL DB
Always in header: 
- Authorization: Bearer [jwttoken]



## /order/basket/list

Authorization: Bearer [jwttoken]

GET

returns
```json
[
  {
    "id": 1,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230c",
    "quantity": "2",
    "price": "3.00",
    "name": "sticker",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  },
  {
    "id": 2,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230d",
    "quantity": "1",
    "price": "3.00",
    "name": "mug",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  },
  {
    "id": 3,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230c",
    "quantity": "2",
    "price": "3.00",
    "name": "sticker",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  }
]
```

## /order/basket/add

Authorization: Bearer [jwttoken]

POST
```json
{
  "catalogId": "6387867a4cf5338b860c230c",
  "quantity": 2
}
```

returns
200 
```json
{
  "status": "success"
}
```
or
404 : error

## /order/main/create

Authorization: Bearer [jwttoken]

order will postback to /update


POST (today can be empty, tomorrow address)
```json
  {
    "name":"",
    "street":"",
    "pobox":"",
    "city":"",
    "postcode":"",
    "country":""
  }
```

return 200
```json
{
  "status": "ok",
  "data": {
    "id": 1,
    "orderName": "Wed Nov 30 2022 21:23",
    "totalPrice": "9.00",
    "status": "unpaid",
    "items": [
      {
        "id": 1,
        "userId": "tdewin",
        "orderId": "1",
        "catalogId": "6387867a4cf5338b860c230c",
        "name": "sticker",
        "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
        "quantity": "2",
        "price": "3.00",
        "totalPrice": "6.00"
      },
      {
        "id": 2,
        "userId": "tdewin",
        "orderId": "1",
        "catalogId": "6387867a4cf5338b860c230d",
        "name": "mug",
        "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
        "quantity": "1",
        "price": "3.00",
        "totalPrice": "3.00"
      }
    ]
  }
}
```

## /order/main/list
Authorization: Bearer [jwttoken]

GET 
```json
[
  {
    "id": 1,
    "orderName": "Wed Nov 30 2022 21:23",
    "status": "unpaid",
    "name": "9.00"
  },
  {
    "id": 2,
    "orderName": "Wed Nov 30 2022 21:34",
    "status": "unpaid",
    "name": "9.00"
  }
]
```
    
## /order/main/list/id
Authorization: Bearer [jwttoken]

GET
returns
```json
{
  "id": "1",
  "name": "Wed Nov 30 2022 21:23",
  "status": "unpaid",
  "totalPrice": "9.00",
  "items": [
    {
      "id": 1,
      "userId": "tdewin",
      "orderId": "1",
      "catalogId": "6387867a4cf5338b860c230c",
      "name": "sticker",
      "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
      "quantity": "2",
      "price": "3.00",
      "totalPrice": "6.00"
    },
    {
      "id": 2,
      "userId": "tdewin",
      "orderId": "1",
      "catalogId": "6387867a4cf5338b860c230d",
      "name": "mug",
      "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
      "quantity": "1",
      "price": "3.00",
      "totalPrice": "3.00"
    }
  ]
}
```
