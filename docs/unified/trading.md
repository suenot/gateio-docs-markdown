# Unified Trading

## Overview

Unified trading allows you to trade across spot, margin, and derivatives markets using a single account. All positions contribute to your portfolio margin requirements.

## Place Order

Place a new order in any supported market.

### HTTP Request

`POST /api/v4/unified/orders`

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| product | string | Yes | Product type: `spot`, `futures`, `options` |
| contract | string | Yes | Trading pair or contract name |
| size | string | Yes | Order size (positive for buy, negative for sell) |
| price | string | Yes | Order price |
| type | string | No | Order type: `limit` or `market` (default: `limit`) |
| tif | string | No | Time in force: `gtc`, `ioc`, `poc` (default: `gtc`) |
| reduce_only | boolean | No | Whether the order can only reduce position |
| auto_borrow | boolean | No | Whether to borrow if insufficient balance |
| text | string | No | Custom order tag |

### Response

```json
{
  "id": "123456789",
  "product": "spot",
  "contract": "BTC_USDT",
  "size": "1.0",
  "price": "50000",
  "filled_size": "0",
  "filled_price": "0",
  "status": "open",
  "type": "limit",
  "tif": "gtc",
  "create_time": 1234567890,
  "finish_time": 0
}
```

## Cancel Order

Cancel an existing order.

### HTTP Request

`DELETE /api/v4/unified/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |
| product | string | Yes | Product type |

### Response

Same as order creation response with updated status

## Get Position

Get current position information.

### HTTP Request

`GET /api/v4/unified/positions/{contract}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | Yes | Trading pair or contract name |
| product | string | Yes | Product type |

### Response

```json
{
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
  "risk_level": "low"
}
```

## Get Order Status

Get status of a specific order.

### HTTP Request

`GET /api/v4/unified/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |
| product | string | Yes | Product type |

### Response

Same as order creation response

## Get Open Orders

Get all open orders.

### HTTP Request

`GET /api/v4/unified/open_orders`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| product | string | No | Product type filter |
| contract | string | No | Trading pair or contract filter |
| limit | integer | No | Maximum number of orders returned |
| offset | integer | No | List offset, starting from 0 |

### Response

List of orders, each in the same format as order creation response

## Risk Management

### Position Limits

1. Maximum position size varies by product
2. Leverage limits based on margin ratio
3. Concentration limits per asset

### Order Controls

1. Price limits relative to mark price
2. Size limits based on available margin
3. Rate limits per user

### Margin Requirements

1. Initial margin: Required to open position
2. Maintenance margin: Minimum to keep position
3. Auto-deleveraging if margin ratio too low

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 5000 | Invalid product | Product type not supported |
| 5001 | Price out of bounds | Order price too far from mark |
| 5002 | Size too large | Exceeds position limit |
| 5003 | Insufficient margin | Not enough available margin |
