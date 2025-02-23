# WebSocket User Data Stream

## Overview

The user data stream provides real-time updates to your account and orders. Authentication is required to use these streams.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Authentication

1. Generate an API key and secret from your Gate.io account
2. Sign your WebSocket connection:

```json
{
  "time": 1234567890,
  "channel": "spot.balances",
  "event": "subscribe",
  "auth": {
    "method": "api_key",
    "KEY": "your_api_key",
    "SIGN": "calculated_signature"
  }
}
```

## Account Updates

Subscribe to account balance changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.balances",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.balances",
  "event": "update",
  "result": {
    "timestamp": "1234567890123",
    "currency": "BTC",
    "available": "1.23456789",
    "locked": "0.12345678"
  }
}
```

## Order Updates

Subscribe to order status changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.orders",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.orders",
  "event": "update",
  "result": {
    "id": "123456789",
    "user": "1234",
    "text": "custom_tag",
    "create_time": "1234567890",
    "update_time": "1234567890",
    "currency_pair": "BTC_USDT",
    "status": "open",
    "type": "limit",
    "account": "spot",
    "side": "buy",
    "amount": "1",
    "price": "50000",
    "filled_amount": "0.5",
    "filled_price": "49999",
    "fee": "0.1",
    "fee_currency": "USDT",
    "point_fee": "0",
    "gt_fee": "0",
    "gt_discount": false,
    "rebated_fee": "0",
    "rebated_fee_currency": "USDT"
  }
}
```

## Personal Trading Updates

Subscribe to your own trades.

### Request

```json
{
  "time": 1234567890,
  "channel": "spot.usertrades",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "spot.usertrades",
  "event": "update",
  "result": {
    "id": "12345",
    "create_time": "1234567890",
    "create_time_ms": "1234567890123",
    "currency_pair": "BTC_USDT",
    "side": "buy",
    "role": "taker",
    "amount": "1.0",
    "price": "50000",
    "order_id": "123456789",
    "fee": "0.1",
    "fee_currency": "USDT",
    "point_fee": "0",
    "gt_fee": "0"
  }
}
```

## Error Handling

If an authentication or subscription error occurs:

```json
{
  "time": 1234567890,
  "channel": "spot.orders",
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
