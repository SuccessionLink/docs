---
title: API Reference

language_tabs:
  - shell
  - python

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Succession Link API
---

# Introduction

Welcome to the Succession Link API. This is a work in progress.

# Authentication

Authentication is done via an API key. Include an "Authorization" header in each
request. The format is as follows:

`Authorization: Token YOUR_TOKEN`


# Valuations

## Quick Valuation

The quick valuation provides a simple valuation based on a minimal number of
attributes. Most attributes are optional. 

The response includes a `valuation` object which contains the value for the
business.

```python
import json
import requests
data = {
    "revenue": {
        "year": 2023,
        "revenue": 10000000
    },
    "business_info": {
        "ownership_percentage" : 100,
        "percent_commission_based" : 10,
        "percent_fee_based" : 90
    }
}

url = "https://successionlink.com/api/v1/valuation/quick/"
r = requests.post(
  url,
  headers={
    'content-type': 'application/json',
    'authorization': 'Token YOUTOKEN'
  }, data=json.dumps(data))
print(json.dumps(r.json(), indent=2))
```


```shell
curl -X POST https://successionlink.com/api/v1/valuation/quick/ \
-H 'Authentication: Token TOKEN' \
-H 'Content-Type: application/json' \
-d @- <<EOF
{
    "revenue": {
        "year": 2023,
        "revenue": 10000000
    },
    "business_info": {
        "ownership_percentage" : 100,
        "percent_commission_based" : 10,
        "percent_fee_based" : 90
    }
}
EOF
```

> The above command returns JSON structured like this:

```json
{
  "status": "success",
  "statusCode": 200,
  "message": "valuations data",
  "response": {
    "default_method": "fee_commission",
    "supported_methods": [
      "fee_commission"
    ],
    "fee_commission": {
      "valuation": 26375000,
      "fee_valuation": 24750000,
      "commission_valuation": 1624999.9999999995,
      "fee_multiplier": 2.75,
      "commission_multiplier": 1.625
    },
    "valuation": {
      "valuation": 26375000,
      "fee_valuation": 24750000,
      "commission_valuation": 1624999.9999999995,
      "fee_multiplier": 2.75,
      "commission_multiplier": 1.625
    }
  }
}
```

### HTTP Request

`POST https://successionlink.com/api/v1/valuation/quick/`

### Data Parameters

Parameter     | Description
------------- | -----------
revenue       | This is an object that contains data on expenses and revenue
business_info | This is an object that contains details on the business

### Revenue Object
Parameter        | Description
-------------    | -----------
year             | The year for the data
revenue          | Total revenue for `year`
aum              | (Optional) The year for the data
expenses         | (Optional) Total expenses for `year`
interest_expense | (Optional) Total interest expense for `year`
noncash_expenses | (Optional) Total non-cash expenses for `year`
office_comp      | (Optional) Officer compensation for `year`

### Business Info Object
Parameter                | Description
-------------            | -----------
ownership_percentage     | The percent ownership for this user. If unsure default to 100%
percent_commission_based | Percentage of revenue that is commission based
percent_fee_based        | Percentage of revenue that is fee based
avg_client_age           | (Optional) Average age of clients
num_clients              | (Optional) Number of cliens
num_employees            | (Optional) Number of employess
business_name            | (Optional) Name of business
street1                  | (Optional) Business address
street2                  | (Optional) Business address 2
city                     | (Optional) Business city
state                    | (Optional) Business state
zip                      | (Optional) Business zip
