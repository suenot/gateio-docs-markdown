# Options WebSocket Market Data Stream

## Overview

The WebSocket feed provides real-time market data updates for options contracts. Connect to the WebSocket endpoint and subscribe to the channels you're interested in.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Subscribe to Tickers

Get real-time price updates for options contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.tickers",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.tickers",
  "event": "update",
  "result": {
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
}
```

## Subscribe to Order Book

Get real-time order book updates for options contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.order_book",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C", "20", "100ms"]
}
```

Parameters:
- Contract name
- Depth limit (number of price levels)
- Update interval ("100ms" or "full")

### Response

```json
{
  "time": 1234567890,
  "channel": "options.order_book",
  "event": "update",
  "result": {
    "t": 1234567890123,
    "s": "BTC_USDT-20230630-30000-C",
    "bids": [["1990", "1.5"], ["1985", "2.0"]],
    "asks": [["2010", "1.0"], ["2015", "2.5"]]
  }
}
```

## Subscribe to Trades

Get real-time trade updates for options contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.trades",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.trades",
  "event": "update",
  "result": {
    "id": 123456,
    "create_time": 1234567890,
    "create_time_ms": 1234567890123,
    "contract": "BTC_USDT-20230630-30000-C",
    "size": "1.0",
    "price": "2000"
  }
}
```

## Subscribe to Greeks

Get real-time Greeks updates for options contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.greeks",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.greeks",
  "event": "update",
  "result": {
    "contract": "BTC_USDT-20230630-30000-C",
    "delta": "0.65",
    "gamma": "0.0001",
    "vega": "10",
    "theta": "-5",
    "implied_volatility": "0.5"
  }
}
```

## Error Handling

If an error occurs, you'll receive an error message:

```json
{
  "time": 1234567890,
  "channel": "options.tickers",
  "event": "error",
  "message": "Invalid subscription format",
  "code": 1
}
```

## Best Practices

1. Maintain a single WebSocket connection for multiple subscriptions
2. Implement reconnection logic with exponential backoff
3. Resubscribe to channels after reconnecting
4. Handle connection errors gracefully
5. Process order book updates sequentially
6. Monitor Greeks for risk management
7. Validate data integrity
8. Consider using compressed data when available
