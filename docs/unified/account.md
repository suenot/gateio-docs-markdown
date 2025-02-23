# Unified Account

## Overview

The unified account combines spot, margin, and derivatives trading under a single portfolio margin system. This allows for more efficient use of capital and better risk management.

## Get Unified Account

Get unified account information and balances.

### HTTP Request

`GET /api/v4/unified/accounts`

### Response

```json
{
  "user_id": 123456,
  "total_equity": "100000",
  "total_liability": "20000",
  "total_margin_balance": "80000",
  "total_initial_margin": "10000",
  "total_maintenance_margin": "5000",
  "total_available_margin": "70000",
  "total_unrealized_pnl": "1000",
  "margin_level": "8.0",
  "portfolio_margin_total_equity": "100000",
  "portfolio_margin_total_liability": "20000",
  "currency": "USDT"
}
```

## Get Account Balance

Get detailed balance information for each asset.

### HTTP Request

`GET /api/v4/unified/asset_balance`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | No | Currency name |

### Response

```json
[
  {
    "currency": "BTC",
    "available": "1.0",
    "locked": "0.5",
    "borrowed": "0.2",
    "interest": "0.0001",
    "total": "1.5",
    "value_in_usd": "45000",
    "value_in_btc": "1.5"
  }
]
```

## Get Position Risk

Get risk metrics for all positions.

### HTTP Request

`GET /api/v4/unified/positions`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| product | string | No | Product type: `spot`, `futures`, `options` |

### Response

```json
{
  "total_risk": "10000",
  "total_margin_requirement": "5000",
  "margin_ratio": "0.2",
  "liquidation_price": "25000",
  "positions": [
    {
      "product": "spot",
      "currency_pair": "BTC_USDT",
      "size": "1.0",
      "value": "30000",
      "leverage": "2",
      "risk_weight": "0.1",
      "margin_requirement": "3000"
    }
  ]
}
```

## Get Borrowing Info

Get borrowing information for margin positions.

### HTTP Request

`GET /api/v4/unified/borrowable`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| currency | string | No | Currency name |

### Response

```json
[
  {
    "currency": "BTC",
    "borrowable": "10.0",
    "interest_rate": "0.0002",
    "min_borrow_amount": "0.01",
    "borrow_ceiling": "100"
  }
]
```

## Portfolio Margin System

### Risk Calculation

The portfolio margin system calculates risk based on:
1. Asset correlations
2. Historical volatility
3. Market liquidity
4. Position concentration

### Margin Requirements

Margin levels are determined by:
1. Initial margin: Required to open positions
2. Maintenance margin: Minimum to maintain positions
3. Liquidation margin: Trigger for position liquidation

### Risk Tiers

| Tier | Margin Ratio | Max Leverage |
|------|--------------|--------------|
| 1    | < 0.1        | 10x         |
| 2    | 0.1 - 0.2    | 5x          |
| 3    | 0.2 - 0.5    | 2x          |
| 4    | > 0.5        | 1x          |

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 4000 | Account not unified | Unified account not activated |
| 4001 | Margin call | Below maintenance margin |
| 4002 | Insufficient balance | Not enough available margin |
| 4003 | Position limit | Maximum position size reached |
