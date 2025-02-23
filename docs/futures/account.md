# Futures Account Endpoints

This document describes the account management endpoints for Gate.io Futures Trading API v4.

## Get Account Info

Retrieve futures account information.

### HTTP Request

`GET /api/v4/futures/accounts`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |

### Response

```json
{
  "user": 1234,
  "total": "10.5",
  "unrealized_pnl": "0.1",
  "margin": "5.2",
  "available": "5.2",
  "point": "0.0",
  "currency": "USDT"
}
```

## Get Position Risk

Retrieve risk metrics for all positions.

### HTTP Request

`GET /api/v4/futures/positions`

### Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| settle    | string | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string | No       | Futures contract name |

### Response

```json
[
  {
    "contract": "BTC_USDT",
    "size": 1,
    "leverage": "10",
    "risk_limit": "100",
    "margin": "5.2",
    "entry_price": "8500.0",
    "liq_price": "7800.0",
    "mark_price": "8600.0",
    "unrealized_pnl": "0.1",
    "maintenance_rate": "0.005"
  }
]
```

## Get Funding History

Retrieve funding payment history.

### HTTP Request

`GET /api/v4/futures/funding_rate/history`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| contract  | string  | Yes      | Futures contract name |
| limit     | integer | No       | Maximum number of records returned (default: 100) |

### Response

```json
[
  {
    "contract": "BTC_USDT",
    "funding_time": 1623456789,
    "funding_rate": "0.002",
    "amount": "0.1"
  }
]
```

## Get Account Book

Retrieve account balance change history.

### HTTP Request

`GET /api/v4/futures/account_book`

### Parameters

| Parameter | Type    | Required | Description |
|-----------|---------|----------|-------------|
| settle    | string  | Yes      | Settlement currency: `btc` or `usdt` |
| limit     | integer | No       | Maximum number of records returned (default: 100) |
| type      | string  | No       | Record type: `dnw` - deposit & withdraw, `pnl` - profit & loss, `fee` - fee, `refr` - referral reward, `fund` - funding |

### Response

```json
[
  {
    "time": 1623456789,
    "change": "0.1",
    "balance": "10.5",
    "type": "pnl",
    "text": "Trading profit"
  }
]
```