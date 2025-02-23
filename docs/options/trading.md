# Options Trading

## Place Order

Create a new options order.

### HTTP Request

`POST /api/v4/options/orders`

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | Yes | Options contract name |
| size | string | Yes | Order size (positive for buy, negative for sell) |
| price | string | Yes | Order price |
| type | string | No | Order type: `limit` or `market` (default: `limit`) |
| tif | string | No | Time in force: `gtc`, `ioc`, `poc` (default: `gtc`) |
| text | string | No | Custom order tag |

### Response

```json
{
  "id": "123456789",
  "contract": "BTC_USDT-20230630-30000-C",
  "create_time": 1623456789,
  "size": "1.0",
  "price": "2000",
  "fill_price": "0",
  "status": "open",
  "tif": "gtc",
  "text": "custom_tag"
}
```

## Cancel Order

Cancel an existing options order.

### HTTP Request

`DELETE /api/v4/options/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |

### Response

Same as order creation response with updated status

## Get Order Status

Get status of a specific options order.

### HTTP Request

`GET /api/v4/options/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |

### Response

```json
{
  "id": "123456789",
  "contract": "BTC_USDT-20230630-30000-C",
  "create_time": 1623456789,
  "size": "1.0",
  "price": "2000",
  "fill_price": "2000",
  "status": "closed",
  "tif": "gtc",
  "text": "custom_tag"
}
```

## Get Position

Get current options positions.

### HTTP Request

`GET /api/v4/options/position`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | No | Options contract name |

### Response

```json
{
  "contract": "BTC_USDT-20230630-30000-C",
  "size": "1.0",
  "entry_price": "2000",
  "mark_price": "2100",
  "realised_pnl": "0",
  "unrealised_pnl": "100",
  "pending_orders": 0
}
```

## Get Open Orders

Get all open options orders.

### HTTP Request

`GET /api/v4/options/orders`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | No | Options contract name |
| limit | integer | No | Maximum number of orders returned |
| offset | integer | No | List offset, starting from 0 |

### Response

```json
[
  {
    "id": "123456789",
    "contract": "BTC_USDT-20230630-30000-C",
    "create_time": 1623456789,
    "size": "1.0",
    "price": "2000",
    "fill_price": "0",
    "status": "open",
    "tif": "gtc",
    "text": "custom_tag"
  }
]
```

## Get Order History

Get historical options orders.

### HTTP Request

`GET /api/v4/options/orders/history`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | No | Options contract name |
| status | string | No | Order status: `finished`, `cancelled` |
| limit | integer | No | Maximum number of orders returned |
| offset | integer | No | List offset, starting from 0 |
| from | integer | No | Start timestamp |
| to | integer | No | End timestamp |

### Response

```json
[
  {
    "id": "123456789",
    "contract": "BTC_USDT-20230630-30000-C",
    "create_time": 1623456789,
    "finish_time": 1623456799,
    "size": "1.0",
    "price": "2000",
    "fill_price": "2000",
    "status": "finished",
    "tif": "gtc",
    "text": "custom_tag"
  }
]
```

## Risk Management

1. Options trading carries significant risk
2. Use appropriate position sizing
3. Monitor Greeks for risk exposure
4. Consider implied volatility when pricing
5. Be aware of time decay (theta)
6. Set stop-loss orders where appropriate

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 2000 | Insufficient balance | Not enough funds to place order |
| 2001 | Position limit exceeded | Maximum position size reached |
| 2002 | Invalid order size | Order size must be multiple of contract multiplier |
| 2003 | Price out of bounds | Order price too far from mark price |
