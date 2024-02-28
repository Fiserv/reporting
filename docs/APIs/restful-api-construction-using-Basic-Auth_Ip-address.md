# Constructing a RESTful API Request using Basic Authentication and IP address check

## Environments
Reporting has different environments that allow the consumption of our RESTful APIs for client development, customer acceptance testing, and production. More information on how HTTP Basic Auth works can be seen in this [wiki page](https://en.wikipedia.org/wiki/Basic_access_authentication). As part of extra security Fiserv allows API calls from certain IP addresses which are shared by the Relationship Manager only. Any API call that comes from other IPs which are not shared by the Relationship Manager will be blocked. It is mandated for any merchant to share their trusted IP ranges, hence we name it as Basic Authentication + Ip address check.

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

### Below are the Headers to be passed
Variable | Type | Length | Description/Values
--- | --- | --- | ---
Content-Type | string | N/A | The content type. Valid Value (application/json)
Api-Key | String | N/A | API Key provided to the merchant associating the requests with their Relationship Manager
Auth-Token-Type | String | N/A | Indicates Authorization type Basic or AccessToken.

### Following is the Type of Authorization tab to be selected and fill following relevant details

For Authorization tab: Select Type as **Basic Auth**. This Basic Auth signature will be generated as Authorization: Basic <signature>, where signature is the Base64 encoding of Key and Secret joined by a single colon :

Variable | Description/Values 
--- |---|
Username | **API Key** provided to the merchant associating the requests with their Relationship Manager 
Password | **API Secret** provided to the merchant associating the requests with their Relationship Manager

### Sample Header

```json
"header": {
"Content-Type": "application/json",
"Api-Key": "{{key}}",
"Auth-Token-Type": "Basic",
"Authorization": "Basic {{Encoded Auth signature}}"
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
      "Api-Key": "{key}",
      "Auth-Token-Type": "Basic" ,
      "Authorization": "Basic {{Encoded Auth signature}}"
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

Construct an [API request](?path=docs/APIs/basicAuthApi-model.md) to use the Reporting APIs.