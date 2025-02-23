# Spot Market Data

This section contains endpoints for retrieving market data for spot trading.

## Get Currency Pairs

`GET /api/v4/spot/currency_pairs`

Retrieve a list of all supported currency pairs for spot trading.

### Parameters

None

### Response

```json
[
  {
    "id": "BTC_USDT",
    "base": "BTC",
    "quote": "USDT",
    "fee": "0.2",
    "min_base_amount": "0.0001",
    "min_quote_amount": "1",
    "amount_precision": 4,
    "precision": 2,
    "trade_status": "tradable"
  }
]
```

## Get Tickers

`GET /api/v4/spot/tickers`

Retrieve ticker information for all currency pairs or a specific pair.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| currency_pair | string | No | Currency pair |

### Response

```json
[
  {
    "currency_pair": "BTC_USDT",
    "last": "45000.00",
    "lowest_24h": "44000.00",
    "highest_24h": "46000.00",
    "change_percentage": "2.5",
    "base_volume": "1000.0000",
    "quote_volume": "45000000.00"
  }
]
```

## Get Order Book

`GET /api/v4/spot/order_book`

Retrieve the order book for a specific currency pair.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| currency_pair | string | Yes | Currency pair |
| limit | integer | No | Number of bids/asks (default: 10, max: 100) |

### Response

```json
{
  "id": 123456789,
  "current": 1627544100,
  "update": 1627544099,
  "asks": [
    ["45001.00", "0.5000"],
    ["45002.00", "0.3000"]
  ],
  "bids": [
    ["44999.00", "0.4000"],
    ["44998.00", "0.2000"]
  ]
}
```

## Get Recent Trades

`GET /api/v4/spot/trades`

Retrieve recent trades for a specific currency pair.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| currency_pair | string | Yes | Currency pair |
| limit | integer | No | Number of trades (default: 100, max: 1000) |

### Response

```json
[
  {
    "id": "123456789",
    "create_time": 1627544100,
    "create_time_ms": "1627544100123",
    "side": "buy",
    "currency_pair": "BTC_USDT",
    "amount": "0.0100",
    "price": "45000.00"
  }
]
```

## Get Candlestick Data

`GET /api/v4/spot/candlesticks`

Retrieve candlestick/kline data for a specific currency pair.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| currency_pair | string | Yes | Currency pair |
| interval | string | No | Time interval (default: 30m) |
| limit | integer | No | Number of candles (default: 100, max: 1000) |

### Supported Intervals

The following intervals are supported for candlestick data:
- `10s`: 10 seconds
- `1m`: 1 minute
- `5m`: 5 minutes
- `15m`: 15 minutes
- `30m`: 30 minutes
- `1h`: 1 hour
- `4h`: 4 hours
- `8h`: 8 hours
- `1d`: 1 day
- `7d`: 7 days

### Response

```json
[
  [
    "1627544100",  // timestamp
    "45000.00",    // open
    "45100.00",    // high
    "44900.00",    // low
    "45050.00",    // close
    "100.0000",    // volume
    "4505000.00"   // quote volume
  ]
]
```

## Rate Limits

The following rate limits apply to market data endpoints:

| Endpoint | Rate Limit |
|----------|------------|
| All Market Endpoints | 900 requests per minute |
| Order Book | 300 requests per minute |
| Recent Trades | 300 requests per minute |
| Candlesticks | 300 requests per minute |

Rate limits are applied per IP address. Exceeding these limits will result in HTTP 429 errors.

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 400 | Bad Request | Invalid request format |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | The request is forbidden |
| 404 | Not Found | The specified resource does not exist |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Internal server error |
| 1001 | Invalid currency pair | The specified currency pair is not supported |
| 1002 | Invalid interval | The specified candlestick interval is not supported |
| 1003 | Invalid limit | The specified limit is out of range |
| 1004 | Market is closed | Trading is currently suspended for this pair |

## Best Practices

1. Use WebSocket for real-time data instead of polling REST endpoints
2. Cache market data locally and update only when needed
3. Implement rate limiting in your client
4. Handle error responses gracefully
5. Use appropriate intervals for candlestick data based on your needs
6. Monitor your API usage to avoid hitting rate limits