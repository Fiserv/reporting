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
### Generation of Hmac signature for both GET/POST API calls

> To ensure data integrity, prevent replay attacks, and eliminate stale requests, Authentication is required as part of the Header.

### Details
- Signature Algorithm: SHA256 HMAC
- Signature Encoding: Base64

The message data for the signature is the following items concatenated: Api-Key, Client-Request-Id, Timestamp, Request-Body, Content-Type.

# Code Example

#### The below code have to be sent in Pre-request Java Script for POST calls

```javascript
var key = 'apiKey';
var secret = 'secret';
var ClientRequestId = Math.floor((Math.random() * 10000000) + 1);
var time = new Date().getTime();
var method = request.method;
var requestBody = request.data;
var rawSignature = key + ClientRequestId + time + requestBody;
var computedHash = CryptoJS.algo.HMAC.create(CryptoJS.algo.SHA256, secret.toString());
computedHash.update(rawSignature);
computedHash = computedHash.finalize();
var computedHmac = b64encode(computedHash.toString());

postman.setEnvironmentVariable('key', key);
postman.setEnvironmentVariable('time', time);
postman.setEnvironmentVariable('signature', computedHmac);
postman.setEnvironmentVariable('ClientRequestId', ClientRequestId)
function b64encode (input) {
var swaps = ["A","B","C","D","E","F","G","H","I","J","K","L","M",
"N","O","P","Q","R","S","T","U","V","W","X","Y","Z",
"a","b","c","d","e","f","g","h","i","j","k","l","m",
"n","o","p","q","r","s","t","u","v","w","x","y","z",
"0","1","2","3","4","5","6","7","8","9","+","/"],
tb, ib = "",
output = "",
i, L;
for (i=0, L = input.length; i < L; i++) {
tb = input.charCodeAt(i).toString(2);
while (tb.length < 8) {
tb = "0"+tb;
}
ib = ib + tb;
while (ib.length >= 6) {
output = output + swaps[parseInt(ib.substring(0,6),2)];
ib = ib.substring(6);
}
}
if (ib.length == 4) {
tb = ib + "00";
output += swaps[parseInt(tb,2)] + "=";
}
if (ib.length == 2) {
tb = ib + "0000";
output += swaps[parseInt(tb,2)] + "==";
}
return output;
}
```

#### The below code have to be sent in Pre-request Java Script for GET calls

```javascript
var key = 'apiKey';
var secret = 'secret';
var ClientRequestId = Math.floor((Math.random() * 10000000) + 1);
var time = new Date().getTime();
var method = request.method;
var rawSignature = key + ClientRequestId + time;
var computedHash = CryptoJS.algo.HMAC.create(CryptoJS.algo.SHA256, secret.toString());
computedHash.update(rawSignature);
computedHash = computedHash.finalize();
var computedHmac = b64encode(computedHash.toString());

postman.setEnvironmentVariable('key', key);
postman.setEnvironmentVariable('time', time);
postman.setEnvironmentVariable('signature', computedHmac);
postman.setEnvironmentVariable('ClientRequestId', ClientRequestId)
function b64encode (input) {
var swaps = ["A","B","C","D","E","F","G","H","I","J","K","L","M",
"N","O","P","Q","R","S","T","U","V","W","X","Y","Z",
"a","b","c","d","e","f","g","h","i","j","k","l","m",
"n","o","p","q","r","s","t","u","v","w","x","y","z",
"0","1","2","3","4","5","6","7","8","9","+","/"],
tb, ib = "",
output = "",
i, L;
for (i=0, L = input.length; i < L; i++) {
tb = input.charCodeAt(i).toString(2);
while (tb.length < 8) {
tb = "0"+tb;
}
ib = ib + tb;
while (ib.length >= 6) {
output = output + swaps[parseInt(ib.substring(0,6),2)];
ib = ib.substring(6);
}
}
if (ib.length == 4) {
tb = ib + "00";
output += swaps[parseInt(tb,2)] + "=";
}
if (ib.length == 2) {
tb = ib + "0000";
output += swaps[parseInt(tb,2)] + "==";
}
return output;
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

Construct an [API request](?path=docs/APIs/api-model.md) to use the Reporting APIs.