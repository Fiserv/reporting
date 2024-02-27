# Constructing a RESTful API Request using Hmac Authentication

## Environments

Reporting has different environments that allow the consumption of our RESTful APIs for client development, customer acceptance testing, and production.

> Reporting highly recommends testing against sandbox credentials before using production environment credentials.

### Sandbox
> https://prod.api.fiservapps.com/reporting{endpoint}

- Mimics the production environment
- Uses Sandbox credentials
- Test APIs before certifying for production
- View Request and Response with sandbox details

### Production 
> https://prod.api.fiservapps.com/reporting{endpoint}

- Uses production credentials
- Actual Data is rendered 
- Access Value Added Services

Variable | Type | Length | Description/Values
--- | --- | --- | ---
Content-Type | string | N/A | The content type. Valid Value (application/json)
Client-Request-Id | String | N/A | A client-generated ID for request tracking and signature creation, unique per request. This is also used for idempotency control. Recommended 128-bit UUID format.
Api-Key | String | N/A | API Key provided to the merchant associating the requests with their Relationship Manager
Timestamp | integer | N/A | Epoch timestamp in milliseconds in the request from a client system. Used for Message Signature generation and time limit (5 mins).
Auth-Token-Type | String | N/A | Indicates Authorization type HMAC or AccessToken.
Authorization | String | N/A |Used to ensure the request has not been tampered with during transmission. Valid encryption; HMAC or AccessToken.

### Sample Header

```json
"header": {
"Content-Type": "application/json",
"Client-Request-Id": "{{ClientRequestId}}",
"Api-Key": "{{key}}",
"Timestamp": "{{time}}",
"Auth-Token-Type": "HMAC" ,
"Authorization": "{{signature}}"
}
```
Please go through this document for [Generation of signature](?path=docs/APIs/hmac-authentication.md) 
## Request Body

The body of the transaction request differs based on the CLX Service you want. Below is the sample body for Authorizations request.

#### Request Body Example

```json
{
"fromDate": "20210801",
"toDate": "20210807",
"limit": 3,
"fields": [
"ApprovalCode",
"AccountNumber",
"Amount",
"Currency",
"PaymentMethod",
"Network"
],
"filters": {
"approvalCodes": [
"Declined"
]
}
}
```

## API Call Example

A standard API call to execute a Authorization Search call might look like this:

```json
{
  method: "POST",
  url: "https://prod.api.fiservapps.com/reporting/v1/authorization/search",
  headers: {
      "Content-Type": "application/json",
      "Client-Request-Id": "{ClientRequestId}",
      "Api-Key": "{key}",
      "Timestamp": "{time}",
      "Auth-Token-Type": "HMAC" ,
      "Authorization": "{signature}"
  },
  body: JSON.stringify({
  "fromDate": "20210801",
  "toDate": "20210807",
  "limit": 3,
  "fields": [
    "ApprovalCode",
    "AccountNumber",
    "Amount",
    "Currency",
    "PaymentMethod",
    "Network"
  ],
  "filters": {
    "approvalCodes": [
      "Declined"
    ]
  }
})
}
```

### Constructing the API call with more examples

Construct an [API request](?path=docs/APIs/api-model.md) to use the Reporting APIs.