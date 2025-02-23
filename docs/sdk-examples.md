# SDK Examples

This guide provides examples of how to use Gate.io's official SDKs. Each SDK provides a convenient way to interact with Gate.io's API v4.

## Python SDK

### Installation

```bash
pip install gate-api
```

### Spot Trading Example

```python
from gate_api import ApiClient, Configuration, Order, SpotApi

# Initialize API client
config = Configuration(
    key='your_api_key',
    secret='your_api_secret'
)
spot_api = SpotApi(ApiClient(config))

# Get currency pair info
currency_pair = "BTC_USDT"
pair = spot_api.get_currency_pair(currency_pair)
min_amount = pair.min_base_amount

# Get last price
tickers = spot_api.list_tickers(currency_pair=currency_pair)
last_price = tickers[0].last

# Check account balance
accounts = spot_api.list_spot_accounts(currency=currency_pair.split("_")[1])
available = accounts[0].available

# Place order
order = Order(
    amount=min_amount * 2,
    price=last_price,
    side='buy',
    currency_pair=currency_pair
)
created = spot_api.create_order(order)

# Check and cancel order if needed
if created.status == 'open':
    order_result = spot_api.get_order(created.id, currency_pair)
    if order_result.left > 0:  # if order not fully filled
        result = spot_api.cancel_order(order_result.id, currency_pair)
```

### Futures Trading Example

```python
from gate_api import ApiClient, Configuration, FuturesApi, FuturesOrder

# Initialize API client
config = Configuration(
    key='your_api_key',
    secret='your_api_secret'
)
futures_api = FuturesApi(ApiClient(config))

# Set up futures trading
settle = "usdt"
contract = "BTC_USDT"

# Set leverage
leverage = "3"
futures_api.update_position_leverage(settle, contract, leverage)

# Get position
try:
    position = futures_api.get_position(settle, contract)
    position_size = position.size
except GateApiException as ex:
    if ex.label != "POSITION_NOT_FOUND":
        raise ex

# Get contract details
futures_contract = futures_api.get_futures_contract(settle, contract)
order_size = max(10, futures_contract.order_size_min)

# Update risk limit
risk_limit = futures_contract.risk_limit_base + futures_contract.risk_limit_step
futures_api.update_position_risk_limit(settle, contract, str(risk_limit))

# Place futures order
order = FuturesOrder(
    contract=contract,
    size=order_size,
    price=price,
    tif='gtc'  # good till cancelled
)
created = futures_api.create_futures_order(settle, order)
```

## Node.js SDK

### Installation

```bash
npm install '@gateio/gate-api'
```

### Basic Usage

```javascript
const GateApi = require('@gateio/gate-api');

const client = new GateApi.ApiClient();
client.setApiKeySecret('YOUR_API_KEY', 'YOUR_API_SECRET');

// Spot API example
const spotApi = new GateApi.SpotApi(client);
const currencyPair = 'BTC_USDT';

// Get market info
spotApi.listTickers(currencyPair)
    .then(tickers => {
        console.log(`Last price: ${tickers[0].last}`);
    })
    .catch(err => console.log(err));

// Place order
const order = new GateApi.Order();
order.currencyPair = currencyPair;
order.amount = '0.01';
order.price = '10000';
order.side = 'buy';

spotApi.createOrder(order)
    .then(response => {
        console.log(`Order created: ${response.id}`);
    })
    .catch(err => console.log(err));
```

## Best Practices

1. **Error Handling**
   - Always implement proper error handling
   - Check for specific error types
   - Handle rate limits appropriately

2. **API Key Management**
   - Store API keys securely
   - Use environment variables
   - Never commit keys to version control

3. **Rate Limiting**
   - Implement exponential backoff
   - Track your API usage
   - Stay within the limits

4. **WebSocket Usage**
   - Use WebSocket for real-time data
   - Implement reconnection logic
   - Handle connection errors

5. **Testing**
   - Test with small amounts first
   - Use testnet when available
   - Validate responses

## Additional Resources

- [Python SDK Repository](https://github.com/gateio/gateapi-python)
- [Node.js SDK Repository](https://github.com/gateio/gateapi-nodejs)
- [API Documentation](https://www.gate.io/docs/developers/apiv4/)