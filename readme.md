## Information
- Please find ```Tasks for QA``` section to see what you need to complete.
- Other sections are the information that you may  find useful to finish the tasks
### Background
- The team is responsible for keeping transactional data and information for 
analyzing demands and providing them to the suppliers.
Previously developers in the team have done setting up and implementation for basic needs
which are ...
    - REST API for adding new orders with input validation (POST)
    - REST API for pulling historical orders' data (GET)
    - PostgreSQL database
    - ```*** You can refer to the service's sample input and output below if needed ***``` 

### Tasks for QA
1. Test plans, test cases or steps to ensure quality of the service before go live
2. Our team business analyst just find out about having data containing ```null``` values when GET orders.
As a QA, what should be the next step?
    ```{
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
   		},
   		{
   			"requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
   			"orderId": "A00003",
   			"purchaseOrderNumber": "POA00003"
   			"shipDate": "2021-12-29T23:04:06Z",
   			"productId": "B02",
   			"productName": "Orange",
   			"createdAt": "2021-12-01T23:04:06Z"
   		},
   		{
   			"requestId": "5c5ad94b-2bd8-4710-b29d-76eb51a70efa",
   			"orderId": "A00004",
   			"purchaseOrderNumber": "POA00004"
   			"shipDate": "2021-12-29T23:04:06Z",
   			"productId": "C01",
   			"productName": null,
   			"createdAt": "2021-12-01T23:04:06Z"
   		}
   		
   		
   	]
   }
    ```
3.
## The sample program completed from developers

### 1. Create REST API for adding new orders with input validation

1.1 Add new orders successfully with valid input
```
POST: localhost:8080/v1/orders

{
    "orders": [
        {
            "orderId": "A00001",
            "purchaseOrderNumber": "POA00001",
            "shipDate": "2021-11-25T23:04:06Z",
            "productId": "A01"
        },
        {
            "orderId": "A00002",
            "purchaseOrderNumber": "POA00002",
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
NOTE: All timestamp fields is in UTC timezone

1.2 Add new orders failed with invalid input
```
POST: localhost:8080/v1/orders

{
    "orders": [
        {
            "orderId": "     ",
            "purchaseOrderNumber": "",
            "shipDate": null,
            "productId": ""
        },
        {
            "orderId": "A0",
            "purchaseOrderNumber": "POA000",
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

### 2. Create PostgreSQL database and tables 

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

NOTE: The service get product name from this mapping table

product_id    |  product_name  |   
------------- | ------------- | 
A01  | Apple
A02  | Orange
B01  | Banana
B02  | Watermelon