# Spot Trading

## Place Order

Creates a new order for spot trading.

### HTTP Request

`POST /api/v4/spot/orders`

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency_pair | string | Yes | Currency pair |
| side | string | Yes | Order side: `buy` or `sell` |
| type | string | No | Order type: `limit` or `market`, default to `limit` |
| account | string | No | Trading account type: `spot`, `margin`, `cross_margin`, default to `spot` |
| amount | string | Yes | Order amount |
| price | string | Yes | Order price, required for `limit` orders |
| time_in_force | string | No | Time in force policy: `gtc`, `ioc`, `fok`, default to `gtc` |
| iceberg | string | No | Amount to display for the iceberg order, `null` or `0` for normal orders |
| auto_borrow | boolean | No | Used in margin or cross margin trading. `true` to borrow automatically if insufficient balance |
| text | string | No | Custom order tag |

### Response

```json
{
  "id": "123456789",
  "text": "custom_tag",
  "create_time": "1548000000",
  "update_time": "1548000100",
  "currency_pair": "BTC_USDT",
  "status": "open",
  "type": "limit",
  "account": "spot",
  "side": "buy",
  "amount": "1",
  "price": "4000",
  "filled_amount": "0",
  "filled_price": "0",
  "fee": "0",
  "fee_currency": "USDT",
  "point_fee": "0",
  "gt_fee": "0",
  "gt_discount": false,
  "rebated_fee": "0",
  "rebated_fee_currency": "USDT"
}
```

## Cancel Order

Cancel an order.

### HTTP Request

`DELETE /api/v4/spot/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |
| currency_pair | string | Yes | Currency pair |

### Response

Same as order creation response

## Get Order Status

Get a single order status.

### HTTP Request

`GET /api/v4/spot/orders/{order_id}`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| order_id | string | Yes | Order ID |
| currency_pair | string | Yes | Currency pair |

### Response

Same as order creation response

## Get Open Orders

Get all open orders.

### HTTP Request

`GET /api/v4/spot/open_orders`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency_pair | string | No | Currency pair |
| account | string | No | Trading account type: `spot`, `margin`, `cross_margin` |
| page | integer | No | Page number |
| limit | integer | No | Maximum number of records returned |

### Response

List of orders, each in the same format as order creation response

## Get Order History

Get order history.

### HTTP Request

`GET /api/v4/spot/orders`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency_pair | string | No | Currency pair |
| account | string | No | Trading account type: `spot`, `margin`, `cross_margin` |
| page | integer | No | Page number |
| limit | integer | No | Maximum number of records returned |
| status | string | No | Order status: `open`, `closed`, `cancelled` |
| side | string | No | Order side: `buy` or `sell` |
| from | integer | No | Start timestamp |
| to | integer | No | End timestamp |

### Response

List of orders, each in the same format as order creation response
