# Futures Trading Endpoints

This document describes the trading endpoints for Gate.io Futures Trading API v4.

## Place Order

Place a new futures order.

### HTTP Request

`POST /api/v4/futures/orders`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string | Yes      | Futures contract name |
| size      | number | Yes      | Order size. Positive for buy, negative for sell |
| price     | string | Yes      | Order price |
| tif       | string | No       | Time in force: `gtc`, `ioc`, `poc` (default: `gtc`) |
| iceberg   | number | No       | Display size for iceberg order |
| text      | string | No       | Custom order ID or remarks |
| reduce_only | boolean | No | Reduce-only order |
| stop | string | No | Stop order type: `loss` or `profit` |
| stop_price | string | No | Stop order trigger price |

### Response

```json
{
  "id": 123456,
  "user": 1234,
  "contract": "BTC_USDT",
  "create_time": 1623456789,
  "size": 1,
  "price": "8500.0",
  "fill_price": "0",
  "left": 1,
  "status": "open",
  "tif": "gtc",
  "is_reduce_only": false,
  "is_close": false
}
```

## Cancel Order

Cancel a specific futures order.

### HTTP Request

`DELETE /api/v4/futures/orders/{order_id}`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| order_id  | string | Yes      | Order ID |

### Response

```json
{
  "id": 123456,
  "user": 1234,
  "contract": "BTC_USDT",
  "create_time": 1623456789,
  "size": 1,
  "price": "8500.0",
  "fill_price": "0",
  "left": 1,
  "status": "cancelled",
  "tif": "gtc",
  "is_reduce_only": false,
  "is_close": false
}
```

## Get Order Status

Retrieve status of a specific futures order.

### HTTP Request

`GET /api/v4/futures/orders/{order_id}`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| order_id  | string | Yes      | Order ID |

### Response

```json
{
  "id": 123456,
  "user": 1234,
  "contract": "BTC_USDT",
  "create_time": 1623456789,
  "size": 1,
  "price": "8500.0",
  "fill_price": "8500.0",
  "left": 0,
  "status": "closed",
  "tif": "gtc",
  "is_reduce_only": false,
  "is_close": false
}
```

## Get Open Orders

Retrieve all open futures orders.

### HTTP Request

`GET /api/v4/futures/orders`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string  | No       | Futures contract name |
| limit     | integer | No       | Maximum number of orders returned (default: 100) |
| offset    | integer | No       | List offset, starting from 0 |

### Response

```json
[
  {
    "id": 123456,
    "user": 1234,
    "contract": "BTC_USDT",
    "create_time": 1623456789,
    "size": 1,
    "price": "8500.0",
    "fill_price": "0",
    "left": 1,
    "status": "open",
    "tif": "gtc",
    "is_reduce_only": false,
    "is_close": false
  }
]

## Order Types

### Reduce-Only Orders

Reduce-only orders can only reduce your position size, never increase it. This is useful for closing positions and managing risk.

```json
{
  "contract": "BTC_USDT",
  "size": -1,
  "price": "50000",
  "reduce_only": true
}
```

- If a reduce-only order would increase your position, it is rejected
- Can be combined with any order type (limit, market)
- Useful for ensuring stop-loss orders don't flip your position

### Stop Orders

Stop orders are triggered when the mark price reaches a specified trigger price.

#### Stop Loss

```json
{
  "contract": "BTC_USDT",
  "size": -1,
  "price": "45000",
  "stop": "loss",
  "stop_price": "46000"
}
```

- Triggers when price moves against your position
- Can be market or limit order
- Recommended for risk management

#### Take Profit

```json
{
  "contract": "BTC_USDT",
  "size": -1,
  "price": "55000",
  "stop": "profit",
  "stop_price": "54000"
}
```

- Triggers when price moves in favor of your position
- Can be market or limit order
- Used to lock in profits

## Position Liquidation

### Liquidation Process

1. Initial warning at 80% of maintenance margin
2. Partial liquidation starts at 90% of maintenance margin
3. Full liquidation at 100% of maintenance margin

### Liquidation Price Calculation

```
Long Position:
Liquidation Price = Entry Price * (1 - Initial Margin Rate + Maintenance Margin Rate)

Short Position:
Liquidation Price = Entry Price * (1 + Initial Margin Rate - Maintenance Margin Rate)
```

### Liquidation Prevention

1. Add margin to position
2. Reduce position size
3. Set stop-loss orders
4. Monitor margin ratio

### Auto-Deleveraging (ADL)

When liquidation cannot be completed in the market:

1. Positions are ranked by profit ratio
2. Profitable counter positions are reduced
3. ADL ranking shown in position details
4. Affected traders compensated at bankruptcy price

## Rate Limits

| Endpoint | Rate Limit |
|----------|------------|
| Order Placement | 100 per second |
| Order Cancellation | 100 per second |
| Position Query | 50 per second |

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 3001 | Insufficient margin | Not enough margin for order |
| 3002 | Position not found | No position exists |
| 3003 | Order rejected | Order parameters invalid |
| 3004 | Reduce only violation | Order would increase position |
| 3005 | Position would exceed limit | Order size too large |
| 3006 | Price deviation too large | Order price too far from mark |

## Best Practices

1. Always use stop-loss orders
2. Monitor liquidation prices
3. Keep sufficient margin buffer
4. Use reduce-only for position closing
5. Consider market impact
6. Monitor ADL rankings
7. Implement proper error handling
8. Track all open orders