# Merchant Reporting Portal

You can build your own integrated merchant portal experience utilizing the APIs available here. Whether it is for a responsive web application or a native mobile application, all the APIs needed to build thee experience are available.
 
Here is a sample Merchant Dashboard we created to show case the APIs. Click here (coming soon) on your favorite browser to see the dashboard live in-action and trace the exact API calls invoked. 

![merchantportal](../../assets/images/merchantportal.png)


#### ![oneicon](../../assets/images/oneicon(1).png)  Funding Summary

Funding summary pulled for the dates specified for the 2 stores. There are many other summary options available, this specific example is pulling the overall summary. Check out the Funding summary API for these options.


<!-- theme: success -->
>**POST** `/v1/funding/summary`

##### Payload

<!--
type: tab
titles: Request, Response
-->

```json
curl -X 'POST' \
  'http://localhost:5005/v1/funding/summary' \
  -H 'accept: application/json' \
  -H 'apiKey: YOUR KEY' \
  -H 'Content-Type: application/json' \
  -d '{
        "fromDate": "20210801",
        "toDate": "20210807"
      }'
```

<!--
type: tab
-->

##### Successful response (200)

```json
[
  {
    "currency": "USD",
    "processedNetSales": 3101.22,
    "processedPaidByOthers": -75.45,
    "processedAdjustments": 0,
    "processedICCharges": -110.1983,
    "processedServiceCharges": 0,
    "processedFees": 0,
    "processedChargebacksReversals": -19.5,
    "processedDeposit": 2979.581,
    "processedAmountPaid": 2979.581
  }
]

```

