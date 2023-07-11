# Security

## API Keys
An API key is a string value passed by a client application to the provisioned API proxies. The key uniquely identifies the client application. The application developer embeds the consumer key in the client application header. The client application must present the consumer key for each request. API Services verifies the consumer key before permitting the applications's request.

## Authentication
Fiserv Reporting API authentication is based on the API keys concept. Each consumer will be assigned their own set of API keys that allow message passing to and from the exposed RESTful interface. Your API keys restrict data to only the owners of that data set. 

Your assigned API keys perform an [HTTP Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication) by passing the assigned key via the request header. 

### Authenticated Request
```javascript
curl -X GET "https://cat.api.firstdata.com/reporting/fraud/search/getMetaData" -H "accept: application/json" -H "apikey: YOURAPIKEY"
```

For more information on creating and using API keys for your organization, please click [here](?path=docs/APIs/restful-api-construction.md) . 

---

## Access Reporting APIs

Get up and running with access to our development portal to use our Reporting APis.

### How to get Keys and Secret?

- To obtain the APIKey and APISecret for accessing the reporting APIs, please reach out to your Fiserv relationship manager.
