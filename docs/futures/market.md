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