# SDK Examples

This guide provides examples of how to use Gate.io's official SDKs in Python, Node.js, and Go. Each SDK provides a convenient way to interact with Gate.io's API v4.

## Python SDK

### Installation

```bash
pip install gate-api
```

### Basic Usage

```python
from gate_api import ApiClient, Configuration, SpotApi
from gate_api.exceptions import GateApiException

# Configure API key authorization
config = Configuration(
    key='YOUR_API_KEY',
    secret='YOUR_API_SECRET'
)

# Create API client
client = ApiClient(config)

# Create spot API instance
spot_api = SpotApi(client)

try:
    # Get currency pairs
    currency_pairs = spot_api.list_currency_pairs()
    print(f"Currency pairs: {currency_pairs}")

    # Get ticker information
    ticker = spot_api.list_tickers(currency_pair='BTC_USDT')
    print(f"BTC_USDT ticker: {ticker}")

except GateApiException as e:
    print(f"Gate API exception: {e}")
except Exception as e:
    print(f"Exception occurred: {e}")
```

## Node.js SDK

### Installation

```bash
npm install gate-api
```

### Basic Usage

```javascript
const { GateApi, ApiClient, SpotApi } = require('gate-api');

// Configure API key authorization
const client = new ApiClient();
client.setApiKeySecret('YOUR_API_KEY', 'YOUR_API_SECRET');

// Create spot API instance
const spotApi = new SpotApi(client);

// Get currency pairs
spotApi.listCurrencyPairs()
    .then(response => {
        console.log('Currency pairs:', response.body);
        return spotApi.listTickers({ currencyPair: 'BTC_USDT' });
    })
    .then(response => {
        console.log('BTC_USDT ticker:', response.body);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

## Go SDK

### Installation

```bash
go get github.com/gateio/gateapi-go/v6
```

### Basic Usage

```go
package main

import (
    "context"
    "fmt"
    "github.com/gateio/gateapi-go/v6"
)

func main() {
    // Configure API key authorization
    client := gateapi.NewClient("YOUR_API_KEY", "YOUR_API_SECRET")

    // Create spot API instance
    spotApi := gateapi.NewSpotApi(client)

    // Get currency pairs
    currencyPairs, _, err := spotApi.ListCurrencyPairs(context.Background())
    if err != nil {
        fmt.Printf("Error getting currency pairs: %v\n", err)
        return
    }
    fmt.Printf("Currency pairs: %v\n", currencyPairs)

    // Get ticker information
    tickers, _, err := spotApi.ListTickers(context.Background(), "BTC_USDT")
    if err != nil {
        fmt.Printf("Error getting ticker: %v\n", err)
        return
    }
    fmt.Printf("BTC_USDT ticker: %v\n", tickers)
}
```

## Rate Limiting

All SDKs implement automatic rate limit handling. Refer to the [Rate Limits](./rate-limits.md) documentation for details on API rate limits.

## Error Handling

All SDKs provide proper error handling mechanisms. Check the [Error Codes](./error-codes.md) documentation for a list of possible API errors.

## Additional Resources

- [Python SDK Repository](https://github.com/gateio/gateapi-python)
- [Node.js SDK Repository](https://github.com/gateio/gateapi-nodejs)
- [Go SDK Repository](https://github.com/gateio/gateapi-go)