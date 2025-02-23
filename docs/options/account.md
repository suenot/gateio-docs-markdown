# Options Account

## Get Account Information

Get options account information and balances.

### HTTP Request

`GET /api/v4/options/accounts`

### Response

```json
{
  "total": "10000",
  "unrealized_pnl": "500",
  "option_value": "2000",
  "short_positions": "3",
  "long_positions": "2",
  "available": "7500",
  "currency": "USDT",
  "margin_ratio": "0.2"
}
```

## Get Position Risk

Get risk metrics for options positions.

### HTTP Request

`GET /api/v4/options/position/risk`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| underlying | string | No | Underlying asset |

### Response

```json
{
  "total_delta": "0.5",
  "total_gamma": "0.001",
  "total_vega": "20",
  "total_theta": "-10",
  "margin_occupied": "2000",
  "margin_required": "1500",
  "effective_leverage": "2.5",
  "portfolio_var": "1000",
  "positions": [
    {
      "contract": "BTC_USDT-20230630-30000-C",
      "size": "1.0",
      "delta": "0.6",
      "gamma": "0.0005",
      "vega": "10",
      "theta": "-5"
    }
  ]
}
```

## Get Account History

Get options account history.

### HTTP Request

`GET /api/v4/options/account/history`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| type | string | No | History type: `premium`, `settlement`, `fee` |
| from | integer | No | Start timestamp |
| to | integer | No | End timestamp |
| limit | integer | No | Maximum number of records returned |
| offset | integer | No | List offset, starting from 0 |

### Response

```json
[
  {
    "time": 1623456789,
    "type": "premium",
    "contract": "BTC_USDT-20230630-30000-C",
    "size": "1.0",
    "amount": "2000",
    "currency": "USDT"
  }
]
```

## Get Settlement History

Get options settlement history.

### HTTP Request

`GET /api/v4/options/settlements`

### Query Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| contract | string | No | Options contract name |
| from | integer | No | Start timestamp |
| to | integer | No | End timestamp |
| limit | integer | No | Maximum number of records returned |
| offset | integer | No | List offset, starting from 0 |

### Response

```json
[
  {
    "time": 1625097600,
    "contract": "BTC_USDT-20230630-30000-C",
    "strike_price": "30000",
    "settlement_price": "35000",
    "profit": "5000",
    "fee": "10",
    "currency": "USDT"
  }
]
```

## Risk Management Tools

### Portfolio Margin

Options accounts use portfolio margin, which considers:
- Delta exposure
- Gamma risk
- Vega risk
- Theta decay
- Correlation between positions

### Margin Requirements

Margin requirements are calculated based on:
1. Initial margin for new positions
2. Maintenance margin for existing positions
3. Premium payments for bought options
4. Collateral for written options

### Risk Metrics

Key risk metrics monitored:
1. Portfolio Value at Risk (VaR)
2. Greeks (Delta, Gamma, Vega, Theta)
3. Margin ratio
4. Effective leverage
5. Position concentration

## Account Limits

| Metric | Limit |
|--------|-------|
| Maximum leverage | 5x |
| Minimum margin ratio | 0.1 |
| Maximum position value | 1000000 USDT |
| Maximum orders per minute | 50 |
| Maximum positions per underlying | 20 |

## Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 3000 | Margin call | Account below maintenance margin |
| 3001 | Account disabled | Trading privileges revoked |
| 3002 | Settlement in progress | Account being settled |
| 3003 | Risk limit exceeded | Position size too large |
