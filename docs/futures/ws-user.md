# Futures WebSocket User Data Stream

## Overview

The user data stream provides real-time updates to your futures account, positions, and orders. Authentication is required to use these streams.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Authentication

1. Generate an API key and secret from your Gate.io account
2. Sign your WebSocket connection:

```json
{
  "time": 1234567890,
  "channel": "futures.orders",
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
  "channel": "futures.positions",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.positions",
  "event": "update",
  "result": {
    "contract": "BTC_USDT",
    "size": 1.0,
    "entry_price": "50000",
    "mark_price": "50100",
    "realised_pnl": "100",
    "unrealised_pnl": "100",
    "leverage": "10",
    "mode": "single"
  }
}
```

## Order Updates

Subscribe to order status changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.orders",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.orders",
  "event": "update",
  "result": {
    "id": "123456789",
    "contract": "BTC_USDT",
    "size": 1.0,
    "price": "50000",
    "fill_price": "49999",
    "status": "closed",
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
  "channel": "futures.balances",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.balances",
  "event": "update",
  "result": {
    "total": "10000",
    "unrealised_pnl": "100",
    "position_margin": "1000",
    "order_margin": "500",
    "available": "8500",
    "point_value": "0",
    "currency": "USDT"
  }
}
```

## Liquidation Updates

Subscribe to liquidation warnings.

### Request

```json
{
  "time": 1234567890,
  "channel": "futures.liquidates",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "futures.liquidates",
  "event": "update",
  "result": {
    "contract": "BTC_USDT",
    "leverage": "10",
    "liq_price": "45000",
    "risk_level": "high"
  }
}
```

## Error Handling

If an authentication or subscription error occurs:

```json
{
  "time": 1234567890,
  "channel": "futures.orders",
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
7. Monitor position and account updates for risk management
8. Set up alerts for liquidation warnings
