# Options WebSocket User Data Stream

## Overview

The user data stream provides real-time updates to your options account, positions, and orders. Authentication is required to use these streams.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Authentication

1. Generate an API key and secret from your Gate.io account
2. Sign your WebSocket connection:

```json
{
  "time": 1234567890,
  "channel": "options.orders",
  "event": "subscribe",
  "auth": {
    "method": "api_key",
    "KEY": "your_api_key",
    "SIGN": "calculated_signature"
  }
}
```

## Position Updates

Subscribe to position changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.positions",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.positions",
  "event": "update",
  "result": {
    "contract": "BTC_USDT-20230630-30000-C",
    "size": "1.0",
    "entry_price": "2000",
    "mark_price": "2100",
    "realised_pnl": "0",
    "unrealised_pnl": "100",
    "delta": "0.65",
    "gamma": "0.0001",
    "vega": "10",
    "theta": "-5"
  }
}
```

## Order Updates

Subscribe to order status changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.orders",
  "event": "subscribe",
  "payload": ["BTC_USDT-20230630-30000-C"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.orders",
  "event": "update",
  "result": {
    "id": "123456789",
    "contract": "BTC_USDT-20230630-30000-C",
    "size": "1.0",
    "price": "2000",
    "fill_price": "2000",
    "status": "closed",
    "type": "limit",
    "tif": "gtc",
    "create_time": 1234567890,
    "finish_time": 1234567890
  }
}
```

## Account Updates

Subscribe to account balance changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.balances",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.balances",
  "event": "update",
  "result": {
    "total": "10000",
    "unrealized_pnl": "500",
    "option_value": "2000",
    "short_positions": "3",
    "long_positions": "2",
    "available": "7500",
    "currency": "USDT",
    "margin_ratio": "0.2"
  }
}
```

## Portfolio Risk Updates

Subscribe to portfolio risk metrics.

### Request

```json
{
  "time": 1234567890,
  "channel": "options.portfolio",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "options.portfolio",
  "event": "update",
  "result": {
    "total_delta": "0.5",
    "total_gamma": "0.001",
    "total_vega": "20",
    "total_theta": "-10",
    "margin_occupied": "2000",
    "margin_required": "1500",
    "effective_leverage": "2.5",
    "portfolio_var": "1000"
  }
}
```

## Error Handling

If an authentication or subscription error occurs:

```json
{
  "time": 1234567890,
  "channel": "options.orders",
  "event": "error",
  "message": "Authentication failed",
  "code": 401
}
```

## Best Practices

1. Maintain a single WebSocket connection for all user data streams
2. Implement reconnection logic with exponential backoff
3. Resubscribe to channels after reconnection
4. Keep track of the last received update to detect missed messages
5. Handle connection errors gracefully
6. Validate received data before processing
7. Monitor Greeks and risk metrics continuously
8. Set up alerts for significant position changes
9. Track portfolio margin requirements
10. Monitor option expiration dates
