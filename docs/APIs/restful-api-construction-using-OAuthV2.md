# Constructing a RESTful API Request using OAuth 2.0 Authentication

## Environments

Reporting has different environments that allow the consumption of our RESTful APIs for client development, customer acceptance testing, and production. As part of OAuth v2 Authentication, Merchant must make 2 API calls. More information on OAuthV2 can be found out on [wiki page](https://en.wikipedia.org/wiki/OAuth)

1.	One for the token generation where the access token is valid only for 59 minutes. After 59 minutes, merchant must generate the access token again by calling the /token/generate API call.
2.	Subsequent call is the actual API Reporting call.

### Below is the Type of Authorization tab to be selected, complete the following relevant details.

We get the token from the field : "access_token" from the response. Once we have the access_token we will be ready to make the actual API call.

## Token Generation API call would be 
> https://prod.api.fiservapps.com/reporting/token/generate
### Below are the Headers to be passed
Variable | Type | Length | Description/Values
--- | --- | --- | ---
Content-Type | string | N/A | The content type. Valid Value (application/x-www-form-urlencoded)
Api-Key | String | N/A | API Key provided to the merchant associating the requests with their Relationship Manager
Auth-Token-Type | String | N/A | Indicates Authorization type OAuthV2.
Client-Request-Id | String | N/A | A client-generated ID for request tracking and signature creation, unique per request. This is also used for idempotency control. Recommended 128-bit UUID format.

### As part of Body: 
Select x-www-form-urlencoded as Body content type which is used to encode form data as Key-value pairs.
Key is **grant_type** and Value is **client_credentials**

### Following is the Type of Authorization tab to be selected and fill following relevant details

For Authorization tab: Select Type as **Basic Auth**. This Basic Auth signature will be generated as Authorization: Basic <signature>, where signature is the Base64 encoding of Key and Secret joined by a single colon :

Variable | Description/Values 
--- |---|
Username | **API Key** provided to the merchant associating the requests with their Relationship Manager 
Password | **API Secret** provided to the merchant associating the requests with their Relationship Manager

As part of Headers: Below are mandatory along with Authorization tab

Api-Key:{{key}}
Auth-Token-Type:OAuthV2
Client-Request-Id:{{ClientRequestId}}

### Sample Header

```json
"header": {
"Content-Type": "application/json",
"Client-Request-Id": "{{ClientRequestId}}",
"Api-Key": "{{key}}",
"Auth-Token-Type": "OAuthV2",
"Authorization": "Basic {{Encoded Auth signature}}"
}
```
From the token API call, we get the response in the below format

```json
{
    "refresh_token_expires_in": "0",
    "api_product_list": "{{string}}",
    "api_product_list_json": [
      "{{string}}"
    ],
    "organization_name": "{{string}}",
    "developer.email": "{{string}}",
    "token_type": "{{string}}",
    "issued_at": "{{string}}",
    "client_id": "{{string}}",
    "access_token": "{{string-Needed access token}}",
    "application_name": "{{string}}",
    "scope": "",
    "expires_in": "3599",
    "refresh_count": "0",
    "status": "approved"
}
```
We get the token from the field : "**access_token**" from response.Once we have the access_token we will be ready to make the Actual API call.
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
Auth-Token-Type | String | N/A | Indicates Authorization type OAuthV2.
Authorization | String | N/A |Used to ensure the request has not been tampered: Bearer {{access_token}}.Here access_token is taken from the first call

### Sample Header

```json
"header": {
"Content-Type": "application/json",
"Client-Request-Id": "{{ClientRequestId}}",
"Api-Key": "{{key}}",
"Auth-Token-Type": "OAuthV2" ,
"Authorization": "Bearer {{access_token}}"
}
```

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

Construct an [API request](?path=docs/APIs/OAuthV2Api-model.md) to use the Reporting APIs.