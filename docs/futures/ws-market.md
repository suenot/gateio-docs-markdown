# Futures WebSocket Market Data Stream

## Overview

The WebSocket feed provides real-time market data updates for futures contracts. Connect to the WebSocket endpoint and subscribe to the channels you're interested in.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Subscribe to Tickers

Get real-time price updates for futures contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.tickers",
  "event": "subscribe",
  "payload": ["BTC_USDT", "ETH_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.tickers",
  "event": "update",
  "result": {
    "contract": "BTC_USDT",
    "last": "50000",
    "index_price": "49995",
    "mark_price": "50005",
    "funding_rate": "0.0001",
    "funding_rate_indicative": "0.0002",
    "volume_24h": "1000",
    "volume_24h_usd": "50000000",
    "volume_24h_btc": "1000",
    "volume_24h_eth": "15000"
  }
}
```

## Subscribe to Order Book

Get real-time order book updates for futures contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.order_book",
  "event": "subscribe",
  "payload": ["BTC_USDT", "20", "100ms"]
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
  "channel": "futures.order_book",
  "event": "update",
  "result": {
    "t": 1234567890123,
    "s": "BTC_USDT",
    "bids": [["50000", "1.5"], ["49999", "2.0"]],
    "asks": [["50001", "1.0"], ["50002", "2.5"]]
  }
}
```

## Subscribe to Trades

Get real-time trade updates for futures contracts.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.trades",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.trades",
  "event": "update",
  "result": {
    "id": 123456,
    "create_time": 1234567890,
    "create_time_ms": 1234567890123,
    "contract": "BTC_USDT",
    "size": 1.0,
    "price": "50000"
  }
}
```

## Subscribe to Funding Rate

Get real-time funding rate updates.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.funding_rate",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.funding_rate",
  "event": "update",
  "result": {
    "contract": "BTC_USDT",
    "funding_rate": "0.0001",
    "funding_rate_indicative": "0.0002",
    "funding_time": 1234567890
  }
}
```

## Error Handling

If an error occurs, you'll receive an error message:

```json
{
  "time": 1234567890,
  "channel": "futures.tickers",
  "event": "error",
  "message": "Invalid subscription format",
  "code": 1
}
```

## Connection Management

- Keep the connection alive by responding to ping messages
- Reconnect if the connection is lost
- Resubscribe to channels after reconnecting
- Handle connection errors gracefully
- Implement rate limiting according to the API guidelines
