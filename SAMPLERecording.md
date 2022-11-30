# GET http://[hostname]:3001/catalog/list
11/30/2022 10:52:02 PM
## Receive
```json
[
  {
    "_id": "6387867a4cf5338b860c230c",
    "name": "sticker",
    "description": "it's a Kasten sticker",
    "price": 3,
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
    "stock": 84
  },
  {
    "_id": "6387867a4cf5338b860c230d",
    "name": "mug",
    "description": "it's a Kasten mug",
    "price": 3,
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
    "stock": 97
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
# GET http://[hostname]:3002/order/basket/list
11/30/2022 10:52:05 PM
## Receive
```json
```
# POST http://[hostname]:3002/order/basket/add
11/30/2022 10:52:10 PM
## Send
```json
{
  "catalogId": "6387867a4cf5338b860c230c",
  "quantity": 2
}
```
## Receive
```json
{
  "status": "success"
}
```
# POST http://[hostname]:3002/order/basket/add
11/30/2022 10:52:14 PM
## Send
```json
{
  "catalogId": "6387867a4cf5338b860c230d",
  "quantity": 1
}
```
## Receive
```json
{
  "status": "success"
}
```
# GET http://[hostname]:3002/order/basket/list
11/30/2022 10:52:19 PM
## Receive
```json
[
  {
    "id": 8,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230c",
    "quantity": "2",
    "price": "3.00",
    "name": "sticker",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  },
  {
    "id": 9,
    "userId": "tdewin",
    "catalogId": "6387867a4cf5338b860c230d",
    "quantity": "1",
    "price": "3.00",
    "name": "mug",
    "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg"
  }
]
```
# GET http://[hostname]:3002/order/main/list
11/30/2022 10:52:23 PM
## Receive
```json
```
# POST http://[hostname]:3002/order/main/create
11/30/2022 10:52:27 PM
## Send
```json
{}
```
## Receive
```json
{
  "status": "ok",
  "data": {
    "id": 6,
    "orderName": "Wed Nov 30 2022 22:52",
    "totalPrice": "9.00",
    "status": "unpaid",
    "items": [
      {
        "id": 12,
        "userId": "tdewin",
        "orderId": "6",
        "catalogId": "6387867a4cf5338b860c230c",
        "name": "sticker",
        "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
        "quantity": "2",
        "price": "3.00",
        "totalPrice": "6.00"
      },
      {
        "id": 13,
        "userId": "tdewin",
        "orderId": "6",
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
# GET http://[hostname]:3002/order/main/list
11/30/2022 10:52:32 PM
## Receive
```json
{
  "id": 6,
  "orderName": "Wed Nov 30 2022 22:52",
  "status": "unpaid",
  "name": "9.00"
}
```
# GET http://[hostname]:3002/order/main/list/6
11/30/2022 10:52:43 PM
## Receive
```json
{
  "id": "6",
  "name": "Wed Nov 30 2022 22:52",
  "status": "unpaid",
  "totalPrice": "9.00",
  "items": [
    {
      "id": 12,
      "userId": "tdewin",
      "orderId": "6",
      "catalogId": "6387867a4cf5338b860c230c",
      "name": "sticker",
      "imgurl": "https://www.kasten.io/hubfs/Kasten%20logos/logo-kasten.io.svg",
      "quantity": "2",
      "price": "3.00",
      "totalPrice": "6.00"
    },
    {
      "id": 13,
      "userId": "tdewin",
      "orderId": "6",
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
# POST http://[hostname]:3002/order/main/pay/6
11/30/2022 10:53:00 PM
## Send
```json
{
  "CVC": "111",
  "K1SA": "1111 2222 3333 4444"
}
```
## Receive
```json
{
  "status": "ok"
}
```
# GET http://[hostname]:3002/order/main/delete/all
11/30/2022 10:53:10 PM
## Receive
```json
{
  "status": "ok"
}
```
# GET http://[hostname]:3002/order/main/list
11/30/2022 10:53:21 PM
## Receive
```json
```
