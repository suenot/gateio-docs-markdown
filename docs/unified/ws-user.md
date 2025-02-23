# Unified WebSocket User Data Stream

## Overview

The unified user data stream provides real-time updates to your unified account, including account balances, positions, and orders across all products. Authentication is required to use these streams.

WebSocket endpoint: `wss://api.gateio.ws/ws/v4`

## Authentication

1. Generate an API key and secret from your Gate.io account
2. Sign your WebSocket connection:

```json
{
  "time": 1234567890,
  "channel": "unified.orders",
  "event": "subscribe",
  "auth": {
    "method": "api_key",
    "KEY": "your_api_key",
    "SIGN": "calculated_signature"
  }
}
```

## Account Updates

Subscribe to unified account balance changes.

### Request

```json
{
  "time": 1234567890,
  "channel": "unified.balances",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "unified.balances",
  "event": "update",
  "result": {
    "total_equity": "100000",
    "total_liability": "20000",
    "total_margin_balance": "80000",
    "total_initial_margin": "10000",
    "total_maintenance_margin": "5000",
    "total_available_margin": "70000",
    "total_unrealized_pnl": "1000",
    "margin_level": "8.0",
    "currency": "USDT",
    "update_time": 1234567890
  }
}
```

## Position Updates

Subscribe to position changes across all products.

### Request

```json
{
  "time": 1234567890,
  "channel": "unified.positions",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "unified.positions",
  "event": "update",
  "result": {
    "product": "futures",
    "contract": "BTC_USDT",
    "size": "1.0",
    "leverage": "2",
    "entry_price": "50000",
    "mark_price": "51000",
    "realised_pnl": "0",
    "unrealised_pnl": "1000",
    "margin_requirement": "5000",
    "maintenance_margin": "2500",
    "risk_level": "low",
    "update_time": 1234567890
  }
}
```

## Order Updates

Subscribe to order status changes across all products.

### Request

```json
{
  "time": 1234567890,
  "channel": "unified.orders",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "unified.orders",
  "event": "update",
  "result": {
    "id": "123456789",
    "product": "spot",
    "contract": "BTC_USDT",
    "size": "1.0",
    "price": "50000",
    "filled_size": "0.5",
    "filled_price": "49999",
    "status": "partial",
    "type": "limit",
    "tif": "gtc",
    "create_time": 1234567890,
    "update_time": 1234567890
  }
}
```

## Risk Updates

Subscribe to portfolio risk updates.

### Request

```json
{
  "time": 1234567890,
  "channel": "unified.risk",
  "event": "subscribe",
  "payload": []
}
```

### Response

```json
{
  "time": 1234567890,
  "channel": "unified.risk",
  "event": "update",
  "result": {
    "total_risk": "10000",
    "total_margin_requirement": "5000",
    "margin_ratio": "0.2",
    "liquidation_risk": "low",
    "largest_position_risk": "3000",
    "portfolio_var": "2000",
    "update_time": 1234567890
  }
}
```

## Error Handling

If an authentication or subscription error occurs:

```json
{
  "time": 1234567890,
  "channel": "unified.orders",
  "event": "error",
  "message": "Authentication failed",
  "code": 401
}
```

## Best Practices

1. Maintain a single WebSocket connection for all unified data streams
2. Implement reconnection logic with exponential backoff
3. Resubscribe to channels after reconnection
4. Keep track of the last received update to detect missed messages
5. Handle connection errors gracefully
6. Validate received data before processing
7. Monitor margin levels continuously
8. Set up alerts for:
   - Low margin ratio
   - Large position changes
   - Significant PnL changes
   - Order fills
9. Process updates in chronological order
10. Maintain local state for faster access
