# GET http://[host]:3001/catalog/list
12/1/2022 11:03:55 AM
## Headers
## Receive
Status: 200
```json
[
  {
    "_id": "6387867a4cf5338b860c230c",
    "name": "sticker",
    "description": "it's a Kasten sticker",
    "price": 3,
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
    "stock": 76
  },
  {
    "_id": "6387867a4cf5338b860c230d",
    "name": "mug",
    "description": "it's a Kasten mug",
    "price": 3,
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
    "stock": 93
  },
  {
    "_id": "6387868a4cf5338b860c230e",
    "name": "lol",
    "description": "lol",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
    "price": 10,
    "stock": 0
  }
]
```
# GET http://[host]:3002/order/basket/list
12/1/2022 11:03:56 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[]
```
# POST http://[host]:3002/order/basket/add
12/1/2022 11:03:56 AM
## Headers
authorization: bearer [token]
## Send
```json
{
  "quantity": 2,
  "catalogId": "6387867a4cf5338b860c230c"
}
```
## Receive
Status: 200
```json
{
  "status": "success"
}
```
# POST http://[host]:3002/order/basket/add
12/1/2022 11:03:57 AM
## Headers
authorization: bearer [token]
## Send
```json
{
  "quantity": 1,
  "catalogId": "6387867a4cf5338b860c230d"
}
```
## Receive
Status: 200
```json
{
  "status": "success"
}
```
# GET http://[host]:3002/order/basket/list
12/1/2022 11:04:02 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[
  {
    "id": 16,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230c",
    "quantity": "2",
    "price": "3.00",
    "name": "sticker",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  },
  {
    "id": 17,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230d",
    "quantity": "1",
    "price": "3.00",
    "name": "mug",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  }
]
```
# DELETE http://[host]:3002/order/basket/delete/16
12/1/2022 11:04:13 AM
## Headers
authorization: bearer [token]
## Send
```json
{}
```
## Receive
Status: 200
```json
{
  "status": "ok"
}
```
# GET http://[host]:3002/order/basket/list
12/1/2022 11:04:21 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[
  {
    "id": 17,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230d",
    "quantity": "1",
    "price": "3.00",
    "name": "mug",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  }
]
```
# GET http://[host]:3002/order/main/list
12/1/2022 11:04:27 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[]
```
# POST http://[host]:3002/order/main/create
12/1/2022 11:04:31 AM
## Headers
authorization: bearer [token]
## Send
```json
{}
```
## Receive
Status: 200
```json
{
  "status": "ok",
  "data": {
    "id": 4,
    "orderName": "Thu Dec 01 2022 10:04",
    "totalPrice": "3.00",
    "status": "unpaid",
    "items": [
      {
        "id": 8,
        "userId": "tdewin",
        "orderId": "4",
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
# GET http://[host]:3002/order/main/list
12/1/2022 11:04:35 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[
  {
    "id": 4,
    "orderName": "Thu Dec 01 2022 10:04",
    "status": "unpaid",
    "name": "3.00"
  }
]
```
# GET http://[host]:3002/order/main/list/4
12/1/2022 11:04:44 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
{
  "id": "4",
  "name": "Thu Dec 01 2022 10:04",
  "status": "unpaid",
  "totalPrice": "3.00",
  "items": [
    {
      "id": 8,
      "userId": "tdewin",
      "orderId": "4",
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
# POST http://[host]:3002/order/main/pay/4
12/1/2022 11:04:51 AM
## Headers
authorization: bearer [token]
## Send
```json
{
  "K1SA": "1111 2222 3333 4444",
  "CVC": "111"
}
```
## Receive
Status: 200
```json
{
  "status": "ok"
}
```
# GET http://[host]:3002/order/main/list
12/1/2022 11:04:55 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[
  {
    "id": 4,
    "orderName": "Thu Dec 01 2022 10:04",
    "status": "paid",
    "name": "3.00"
  }
]
```
# DELETE http://[host]:3002/order/main/delete/4
12/1/2022 11:05:05 AM
## Headers
authorization: bearer [token]
## Send
```json
{}
```
## Receive
Status: 200
```json
{
  "status": "ok"
}
```
# GET http://[host]:3002/order/main/list
12/1/2022 11:05:11 AM
## Headers
authorization: bearer [token]
## Receive
Status: 200
```json
[]
```
