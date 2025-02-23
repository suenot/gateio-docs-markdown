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
```