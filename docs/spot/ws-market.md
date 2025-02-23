# WebSocket Market Data Stream

## Overview

The WebSocket feed provides real-time market data updates. Connect to the WebSocket endpoint and subscribe to the channels you're interested in.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Subscribe to Tickers

Get real-time price updates for trading pairs.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.tickers",
  "event": "subscribe",
  "payload": ["BTC_USDT", "ETH_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.tickers",
  "event": "update",
  "result": {
    "currency_pair": "BTC_USDT",
    "last": "50000",
    "lowest_ask": "50001",
    "highest_bid": "49999",
    "change_percentage": "5.12",
    "base_volume": "1000",
    "quote_volume": "50000000",
    "high_24h": "51000",
    "low_24h": "49000"
  }
}
```

## Subscribe to Order Book

Get real-time order book updates.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.order_book",
  "event": "subscribe",
  "payload": ["BTC_USDT", "20", "100ms"]
}
```

Parameters:
- Currency pair
- Depth limit (number of price levels)
- Update interval ("100ms" or "full")

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.order_book",
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

Get real-time trade updates.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.trades",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.trades",
  "event": "update",
  "result": {
    "id": 123456,
    "create_time": 1234567890,
    "create_time_ms": 1234567890123,
    "side": "buy",
    "currency_pair": "BTC_USDT",
    "amount": "1.0",
    "price": "50000"
  }
}
```

## Subscribe to Candlestick Data

Get real-time candlestick/kline updates.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.candlesticks",
  "event": "subscribe",
  "payload": ["BTC_USDT", "1m"]
}
```

Intervals: "10s", "1m", "5m", "15m", "30m", "1h", "4h", "8h", "1d", "7d", "30d"

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.candlesticks",
  "event": "update",
  "result": {
    "t": "1234567890",
    "v": "1000",
    "c": "50000",
    "h": "50100",
    "l": "49900",
    "o": "49950",
    "n": "1m",
    "a": "50000000"
  }
}
```

## Error Handling

If an error occurs, you'll receive an error message:

```json
{
  "time": 1234567890,
  "channel": "spot.tickers",
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
