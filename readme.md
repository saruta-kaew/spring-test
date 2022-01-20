# Create REST API with JAVA Spring Boot
## Information
### Pre-requisites
- IntelliJ or any code editor https://www.jetbrains.com/idea/download/
- Java 8 https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
- Gradle https://gradle.org/install/
- Lombok https://projectlombok.org/download
- Docker https://hub.docker.com/editions/community/docker-ce-desktop-mac
- PgAdmin https://www.pgadmin.org/download/pgadmin-4-macos/
- Postman https://www.postman.com/downloads/

### Database
- PostgreSQL

### Submission
- Create a new branch with your name-surname-date and commit your changes.

## Tasks

### 1. Create REST API for adding new orders with input validation

1.1 Add new orders successfully with valid input
```
POST: localhost:8080/v1/orders

{
    "orders": [
        {
            "orderId": "A00001",
            "purchaseOrderNumber": "POA00001"
            "shipDate": "2021-11-25T23:04:06Z",
            "productId": "A01"
        },
        {
            "orderId": "A00002",
            "purchaseOrderNumber": "POA00002"
            "shipDate": "2021-11-29T23:04:06Z",
            "productId": "A02"
        }
    ]
}
```
Response when creating successfully with status 201 created
```
{
    "requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
    "createdAt": "2021-12-01T23:04:06Z"
}

```

1.2 Add new orders failed with invalid input
```
POST: localhost:8080/v1/orders

{
    "orders": [
        {
            "orderId": "     ",
            "purchaseOrderNumber": ""
            "shipDate": null,
            "productId": ""
        },
        {
            "orderId": "A0",
            "purchaseOrderNumber": "POA000"
            "shipDate": "2021-11-29",
            "productId": "A"
        }
    ]
}
```
Response when creating failed with status 400 Bad Request
```
{
  "requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
  "errors": [
    {
      "type": "VALIDATION_FAILED",
      "description": "One or more fields specified in the request failed validation",
      "details": [
        {
          "field": "orders[0].orderId",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[6]"
        },
        {
          "field": "orders[0].purchaseOrderNumber",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[8]"
        },
        {
          "field": "orders[0].shipDate",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be YYYY-MM-DD'T'HH:MM:SS'Z' (UTC Format)"
        },
        {
          "field": "orders[0].productId",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[3]"
        },
        {
          "field": "orders[1].orderId",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[6]"
        },
        {
          "field": "orders[1].purchaseOrderNumber",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[8]"
        },
        {
          "field": "orders[1].shipDate",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be YYYY-MM-DD'T'HH:MM:SS'Z' (UTC Format)"
        },
        {
          "field": "orders[1].productId",
          "code": "incorrect_value",
          "message": "This field is mandatory and the format must be char[3]"
        }
      ]
    }
  ]
}

```

### 2. Create PostgreSQL database and tables to persist data using docker-compose and liquibase

2.1 Persist data from valid input

request_id  | order_id  |   purchase_order_number |   ship_date | product_id | created_at |
------------- | ------------- | ------------- | ------------- | ------------- | -------------
5c5ad94b-2bd8-4710-b29d-76eb51a70efa  | A00001  | POA00001  | 2021-11-29 23:04:06+00  |  A01  |  2021-12-01 23:04:06+00
5c5ad94b-2bd8-4710-b29d-76eb51a70efa  | A00002  |  POA00002 | 2021-12-29 23:04:06+00  |  A02  |  2021-12-01 23:04:06+00

### 3. Create REST API to GET orders

3.1 GET all orders successfully with status 200 OK

```
GET: localhost:8080/v1/orders

{
	"orders": [
		{
			"requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
			"orderId": "A00001",
			"purchaseOrderNumber": "POA00001"
			"shipDate": "2021-11-29T23:04:06Z",
			"productId": "A01",
			"productName": "Apple",
			"createdAt": "2021-12-01T23:04:06Z"
		},
		{
			"requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
			"orderId": "A00002",
			"purchaseOrderNumber": "POA00002"
			"shipDate": "2021-12-29T23:04:06Z",
			"productId": "A02",
			"productName": "Orange",
			"createdAt": "2021-12-01T23:04:06Z"
		}
	]
}
```
Response when orders not found
```
{
	"orders": []
}
```

3.2 GET specific order successfully with status 200 OK

```
GET: localhost:8080/v1/order?filter[order][id]=A00001

{
    "requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
    "orderId": "A00001",
    "purchaseOrderNumber": "POA00001"
    "shipDate": "2021-11-29T23:04:06Z",
    "productId": "A01",
    "productName": "Apple",
    "createdAt": "2021-12-01T23:04:06Z"
}
	
```

Response when order not found
```
{}
```

NOTE: Get product name from this mapping table

product_id    |  product_name  |   
------------- | ------------- | 
A01  | Apple
A02  | Orange
B01  | Banana
B02  | Watermelon

### 4. Please list on what and how can we improve?
Example: Add authorization for ordering a product