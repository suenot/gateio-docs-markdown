# Spot Account

## Get Account Balance

Get account balance for spot trading.

### HTTP Request

`GET /api/v4/spot/accounts`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | No | Currency name |

### Response

```json
[
  {
    "currency": "BTC",
    "available": "1.23456789",
    "locked": "0.12345678"
  }
]
```

## Get Account History

Get account history.

### HTTP Request

`GET /api/v4/spot/account_book`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | No | Currency name |
| from | integer | No | Start timestamp |
| to | integer | No | End timestamp |
| page | integer | No | Page number |
| limit | integer | No | Maximum number of records returned |
| type | string | No | Type of history: `deposit`, `withdraw`, `trade` |

### Response

```json
[
  {
    "id": "12345",
    "currency": "BTC",
    "type": "trade",
    "amount": "0.1",
    "time": "1548000000"
  }
]
```

## Account Types

Gate.io supports different types of accounts for trading:

1. **Spot Account (`spot`)**
   - Default trading account
   - Used for regular spot trading

2. **Margin Account (`margin`)**
   - Used for margin trading on specific currency pairs
   - Requires margin account activation

3. **Cross Margin Account (`cross_margin`)**
   - Used for cross margin trading
   - Shares collateral across all positions
   - Requires cross margin account activation

4. **Unified Account**
   - Combines spot, margin, and derivatives trading
   - Provides portfolio margin benefits
   - Requires unified account activation

When placing orders or querying account information, specify the account type using the `account` parameter where applicable.
