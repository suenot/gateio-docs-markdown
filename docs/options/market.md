# Options Market Data

## Get Contracts

Get list of options contracts.

### HTTP Request

`GET /api/v4/options/contracts`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| underlying | string | No | Underlying asset, e.g., `BTC_USDT` |
| expiration | integer | No | Unix timestamp of expiration time |

### Response

```json
[
  {
    "name": "BTC_USDT-20230630-30000-C",
    "underlying": "BTC_USDT",
    "strike_price": "30000",
    "expiration": 1625097600,
    "type": "call",
    "multiplier": "0.01",
    "is_active": true
  }
]
```

## Get Tickers

Get options market tickers.

### HTTP Request

`GET /api/v4/options/tickers`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | No | Options contract name |

### Response

```json
{
  "name": "BTC_USDT-20230630-30000-C",
  "last": "2000",
  "mark_price": "2005",
  "index_price": "29000",
  "bid1_price": "1990",
  "bid1_size": "1.0",
  "ask1_price": "2010",
  "ask1_size": "1.0",
  "volume_24h": "100",
  "volume_24h_usd": "200000",
  "delta": "0.65",
  "gamma": "0.0001",
  "vega": "10",
  "theta": "-5"
}
```

## Get Order Book

Get options order book.

### HTTP Request

`GET /api/v4/options/order_book`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | Yes | Options contract name |
| limit | integer | No | Order book depth limit (default: 10) |

### Response

```json
{
  "contract": "BTC_USDT-20230630-30000-C",
  "asks": [
    ["2010", "1.0"],
    ["2015", "2.0"]
  ],
  "bids": [
    ["1990", "1.5"],
    ["1985", "2.5"]
  ]
}
```

## Get Recent Trades

Get recent options trades.

### HTTP Request

`GET /api/v4/options/trades`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | Yes | Options contract name |
| limit | integer | No | Number of trades returned (default: 100) |

### Response

```json
[
  {
    "id": 123456,
    "create_time": 1623456789,
    "contract": "BTC_USDT-20230630-30000-C",
    "size": "1.0",
    "price": "2000"
  }
]
```

## Get Options Statistics

Get options market statistics.

### HTTP Request

`GET /api/v4/options/underlying/statistics`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| underlying | string | Yes | Underlying asset |

### Response

```json
{
  "underlying": "BTC_USDT",
  "total_calls_volume": "1000",
  "total_puts_volume": "800",
  "total_calls_amount": "2000000",
  "total_puts_amount": "1600000",
  "put_call_ratio": "0.8",
  "index_price": "29000",
  "volatility": "0.65"
}
```

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 1000 | Invalid contract | Contract does not exist |
| 1001 | Invalid underlying | Underlying asset not supported |
| 1002 | Rate limit exceeded | Too many requests |
| 1003 | System error | Internal server error |

## Notes

1. Options contracts follow the naming convention: `{underlying}-{expiration}-{strike}-{type}`
2. Expiration timestamps are in Unix time (seconds)
3. Prices are quoted in settlement currency (USDT)
4. Greeks (delta, gamma, vega, theta) are provided for risk management
5. Volume is in contracts, amount is in settlement currency
