# Futures Market Data Endpoints

This document describes the market data endpoints for Gate.io Futures Trading API v4.

## Get Contracts

Retrieve a list of all futures contracts.

### HTTP Request

`GET /api/v4/futures/contracts`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |

### Response

```json
{
  "name": "BTC_USDT",
  "type": "direct",
  "quanto_multiplier": "0.0001",
  "leverage_min": "1",
  "leverage_max": "100",
  "maintenance_rate": "0.005",
  "mark_type": "index",
  "maker_fee_rate": "-0.00025",
  "taker_fee_rate": "0.00075",
  "order_price_round": "0.1",
  "mark_price_round": "0.1",
  "funding_rate": "0.002",
  "order_size_min": 1
}
```

## Get Tickers

Retrieve ticker information for futures contracts.

### HTTP Request

`GET /api/v4/futures/tickers`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string | No       | Futures contract name |

### Response

```json
{
  "contract": "BTC_USDT",
  "last": "8600.0",
  "change_percentage": "3.5",
  "volume": "10.5",
  "low_24h": "8200.0",
  "high_24h": "8800.0",
  "mark_price": "8500.0",
  "funding_rate": "0.002",
  "index_price": "8500.0"
}
```

## Get Order Book

Retrieve order book data for a specific futures contract.

### HTTP Request

`GET /api/v4/futures/order_book`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string  | Yes      | Futures contract name |
| limit     | integer | No       | Maximum number of order book records (default: 10) |

### Response

```json
{
  "asks": [
    ["8700.0", "0.1"],
    ["8701.0", "0.2"]
  ],
  "bids": [
    ["8699.0", "0.3"],
    ["8698.0", "0.4"]
  ]
}
```

## Get Recent Trades

Retrieve recent trades for a specific futures contract.

### HTTP Request

`GET /api/v4/futures/trades`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string  | Yes      | Futures contract name |
| limit     | integer | No       | Maximum number of trades returned (default: 100) |

### Response

```json
[
  {
    "id": 123456,
    "create_time": 1623456789,
    "contract": "BTC_USDT",
    "size": 0.1,
    "price": "8600.0",
    "side": "buy"
  }
]
```

## Get Candlestick Data

Retrieve candlestick (k-line) data for a specific futures contract.

### HTTP Request

`GET /api/v4/futures/candlesticks`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string  | Yes      | Futures contract name |
| interval  | string  | Yes      | Interval of candlestick: `1m`, `5m`, `15m`, `30m`, `1h`, `4h`, `8h`, `1d`, `7d` |
| limit     | integer | No       | Maximum number of candlesticks returned (default: 100) |

### Response

```json
[
  [
    1623456789,  // timestamp
    "8500.0",    // open
    "8600.0",    // close
    "8700.0",    // high
    "8400.0",    // low
    "100.0"      // volume
  ]
]
```

## Get Funding Rate

Retrieve funding rate for a specific futures contract.

### HTTP Request

`GET /api/v4/futures/funding_rate`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string | Yes      | Futures contract name |

### Response

```json
{
  "funding_rate": "0.002",
  "funding_time": 1623456789
}
```

## Index Price Calculation

The index price is calculated using a weighted average of prices from major spot exchanges. This helps ensure fair and manipulation-resistant price discovery.

### Index Components

- Weighted average of spot prices from major exchanges
- Minimum of 3 exchanges used in calculation
- Outlier prices are excluded
- Updated every second

### Mark Price

The mark price is used for liquidation and PNL calculations. It is calculated using:
1. Index price
2. Funding rate
3. Market basis
4. Insurance fund coefficient

## Funding Rate Mechanism

Funding rates are used to keep perpetual futures prices aligned with the underlying index.

### Calculation

The funding rate consists of two components:
1. Interest Rate Component (IRC)
   - Based on interest rate difference between quote and base currencies
2. Premium Index Component (PIC)
   - Based on the difference between mark price and index price

### Schedule

- Funding is exchanged every 8 hours
- Timestamps: 00:00, 08:00, 16:00 UTC
- Funding rate is capped at ±0.75% per period

### Formula

```
Funding Rate = IRC + PIC
where:
PIC = (Mark Price - Index Price) / Index Price
```

## Liquidity Limits

To ensure market stability and prevent manipulation, the following limits apply:

### Order Size Limits

| Contract Type | Maximum Order Size | Maximum Position Size |
|--------------|-------------------|---------------------|
| BTC_USDT | 1000 contracts | 5000 contracts |
| ETH_USDT | 5000 contracts | 25000 contracts |
| Other | Varies by contract | Varies by contract |

### Price Limits

- Maximum price deviation from mark price: ±5%
- Maximum price deviation during volatile periods: ±10%
- Circuit breaker triggers at ±20% price movement

### Market Impact

- Large orders are split into smaller chunks
- Slippage increases with order size
- Market makers receive incentives for providing liquidity

## Rate Limits

| Endpoint | Rate Limit |
|----------|------------|
| All Market Endpoints | 900 requests per minute |
| Order Book | 300 requests per minute |
| Recent Trades | 300 requests per minute |
| Candlesticks | 300 requests per minute |

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 400 | Bad Request | Invalid request format |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | The request is forbidden |
| 404 | Not Found | The specified resource does not exist |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Internal server error |
| 2001 | Invalid contract | The specified contract is not supported |
| 2002 | Invalid interval | The specified interval is not supported |
| 2003 | Invalid limit | The specified limit is out of range |
| 2004 | Market is closed | Trading is currently suspended |

## Best Practices

1. Use WebSocket for real-time data
2. Monitor funding rates before position entry
3. Consider market impact for large orders
4. Implement proper rate limiting
5. Handle error responses gracefully
6. Cache market data when appropriate
7. Monitor index price components
8. Track liquidation risks