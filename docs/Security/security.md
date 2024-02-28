# Security

## API Keys
An API key is a string value passed by a client application to the provisioned API proxies. The key uniquely identifies the client application. The application developer embeds the consumer key in the client application header. The client application must present the consumer key for each request. API Services verifies the consumer key before permitting the applications's request.

## Authentication
Fiserv Reporting API authentication is based on the API keys concept. Each consumer will be assigned their own set of API keys that allow message passing to and from the exposed RESTful interface. Your API keys restrict data to only the owners of that data set. 

Your assigned API key and Secret can be used to access Reporting API Keys via three methods of Authentication

1. Hmac Authentication 
2. Basic Authentication + IP address check
3. OAuth 2.0 Authentication 

### Hmac Authenticated Request
```javascript
curl -X GET "https://cat.api.firstdata.com/reporting/fraud/search/getMetaData" -H "accept: application/json" -H "apikey: YOURAPIKEY"
```

For more information on creating and using API keys for your organization using: 
1. Hmac Authentication, please click [here](?path=docs/APIs/restful-api-construction-using-Hmac.md)
2. Basic Authentication + IP address please click [here](?path=docs/APIs/restful-api-construction-using-Basic-Auth_Ip-address.md)
3. OAuth 2.0 Authentication please click [here](?path=docs/APIs/restful-api-construction-using-OAuthV2.md)
    
---

### How to get Keys and Secret?

- To obtain the APIKey and APISecret for accessing the reporting APIs, please reach out to your Fiserv relationship manager.
