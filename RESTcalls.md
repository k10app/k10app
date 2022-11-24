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

## /catalog/list/<id> (call back from order)
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

## /catalog/update (call from order svc)
***security issue***

POST
```
[
    {
        "id":1,
        "quantity":1
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
Redis (optional)

## /order/basket/list

Authorization: Bearer <jwttoken>

GET

returns
```
[
    {
        "id":1,
        "quantity":1
        "name":"name",
        "imgurl":"https://",
    }
]
```

## /order/basket/add

Authorization: Bearer <jwttoken>

POST
```
    {
        "id":1,
        "quantity":1
    }
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

## /order/create

Authorization: Bearer <jwttoken>

order will postback to /update


POST
```
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

{
    "id":""
}

## /order/list

GET 
```
{
    id:""
    items:[
        {
                "id":1,
                "quantity":1
                "name":"name",
                "imgurl":"https://",
        }       
    ]
}
```
