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