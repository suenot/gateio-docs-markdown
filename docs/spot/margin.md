# Margin Trading

## Get Margin Account

Get margin account details.

### HTTP Request

`GET /api/v4/margin/accounts`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency_pair | string | No | Currency pair |

### Response

```json
{
  "currency_pair": "BTC_USDT",
  "base": {
    "currency": "BTC",
    "available": "1",
    "locked": "0",
    "borrowed": "0",
    "interest": "0"
  },
  "quote": {
    "currency": "USDT",
    "available": "10000",
    "locked": "0",
    "borrowed": "0",
    "interest": "0"
  }
}
```

## Borrow

Borrow funds for margin trading.

### HTTP Request

`POST /api/v4/margin/loans`

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency name |
| amount | string | Yes | Amount to borrow |
| currency_pair | string | No | Currency pair (required for isolated margin) |

### Response

```json
{
  "id": "12345",
  "create_time": "1548000000",
  "currency": "BTC",
  "amount": "1",
  "status": "open",
  "repaid": "0",
  "repaid_interest": "0",
  "unpaid_interest": "0"
}
```

## Repay

Repay borrowed margin funds.

### HTTP Request

`POST /api/v4/margin/loans/{loan_id}/repayment`

### URL Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| loan_id | string | Yes | Loan ID |

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency name |
| amount | string | Yes | Amount to repay |
| currency_pair | string | No | Currency pair (required for isolated margin) |

### Response

```json
{
  "id": "12345",
  "loan_id": "12345",
  "create_time": "1548000000",
  "currency": "BTC",
  "amount": "1",
  "interest": "0.0001"
}
```

## Transfer

Transfer funds between spot and margin accounts.

### HTTP Request

`POST /api/v4/margin/transfers`

### Request Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | Yes | Currency name |
| amount | string | Yes | Amount to transfer |
| from | string | Yes | Source account: `spot` or `margin` |
| to | string | Yes | Target account: `spot` or `margin` |
| currency_pair | string | No | Currency pair (required for isolated margin) |

### Response

```json
{
  "id": "12345",
  "create_time": "1548000000",
  "currency": "BTC",
  "amount": "1",
  "from": "spot",
  "to": "margin"
}
```

## Cross Margin

Cross margin trading allows you to use your entire margin balance as collateral for any margin trade. This differs from isolated margin where collateral is isolated to specific currency pairs.

To use cross margin:

1. Enable cross margin trading in your account settings
2. When placing orders, set `account=cross_margin`
3. For transfers and loans, omit the `currency_pair` parameter

Cross margin provides more flexible leverage but also carries higher risk as losses in one position can affect your entire margin balance.
