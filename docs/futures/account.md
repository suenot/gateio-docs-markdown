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

## Risk Management System

### Margin Levels

The system uses three key margin levels:

1. Initial Margin (IM)
   - Required to open a position
   - Calculated based on leverage and position size
   - Higher for larger positions

2. Maintenance Margin (MM)
   - Minimum margin to keep position open
   - Usually 50% of initial margin
   - Varies by contract and position size

3. Liquidation Margin (LM)
   - Point at which liquidation begins
   - Equal to maintenance margin
   - Buffer recommended above this level

### Margin Ratio Calculation

```
Margin Ratio = (Wallet Balance + Unrealized PNL) / Initial Margin

Risk Levels:
Safe: > 1.5
Warning: 1.1 - 1.5
Danger: 1.0 - 1.1
Liquidation: < 1.0
```

### Risk Limits

| Contract | Base Risk Limit | Step Size | Maximum Risk Limit |
|----------|----------------|-----------|-------------------|
| BTC_USDT | 100 BTC | 25 BTC | 1000 BTC |
| ETH_USDT | 1000 ETH | 250 ETH | 10000 ETH |

Risk limits determine maximum position size and margin requirements.

## Insurance Fund

The insurance fund protects traders from negative account balances and ensures orderly liquidations.

### Fund Sources

1. Liquidation profits
2. ADL compensation fees
3. Platform contributions

### Fund Usage

1. Cover liquidation losses
2. Maintain market stability
3. Prevent socialized losses

### Fund Statistics

```json
{
  "total_balance": "1000000",
  "daily_income": "5000",
  "daily_usage": "2000",
  "currency": "USDT"
}
```

## Position Risk Calculation

### Value at Risk (VaR)

```
VaR = Position Size * Mark Price * VaR Coefficient

where:
VaR Coefficient is based on:
- Historical volatility
- Market liquidity
- Position concentration
```

### Stress Testing

Positions are stress tested against:
1. Market crashes (-40%)
2. Flash crashes (-20% in 5 minutes)
3. Liquidity crises
4. Volatility spikes

### Risk Metrics

```json
{
  "position_risk": {
    "var_99": "10000",
    "expected_shortfall": "15000",
    "stress_test_loss": "20000"
  }
}
```

## Rate Limits

| Endpoint | Rate Limit |
|----------|------------|
| Account Query | 10 per second |
| Position Risk | 10 per second |
| Account Book | 10 per second |

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 4001 | Insufficient balance | Not enough funds |
| 4002 | Risk limit exceeded | Position too large |
| 4003 | Margin ratio too low | Add margin or reduce position |
| 4004 | Liquidation in progress | Position being liquidated |

## Best Practices

1. Maintain healthy margin ratio
2. Monitor risk limits
3. Use appropriate leverage
4. Set alerts for margin levels
5. Understand liquidation process
6. Keep emergency funds ready
7. Monitor insurance fund status
8. Regular risk assessment