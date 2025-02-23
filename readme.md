# Gate.io API v4 Documentation

## Getting Started
- [Introduction](./docs/introduction.md)
- [Authentication](./docs/authentication.md)
- [Error Codes](./docs/error-codes.md)
- [Rate Limits](./docs/rate-limits.md)

## SDK Examples
- [Python SDK](https://github.com/gateio/gateapi-python)
- [Node.js SDK](https://github.com/gateio/gateapi-nodejs)
- [Go SDK](https://github.com/gateio/gateapi-go)

## Spot Trading
### REST API
- [Market Data](./docs/spot/market.md)
  - Get Currency Pairs
  - Get Tickers
  - Get Order Book
  - Get Recent Trades
  - Get Candlestick Data
- [Trading](./docs/spot/trading.md)
  - Place Order
  - Cancel Order
  - Get Order Status
  - Get Open Orders
  - Get Order History
- [Account](./docs/spot/account.md)
  - Get Account Balance
  - Get Account History
- [Margin Trading](./docs/spot/margin.md)
  - Get Margin Account
  - Borrow
  - Repay
  - Transfer

### WebSocket API
- [Market Data Stream](./docs/spot/ws-market.md)
  - Subscribe to Tickers
  - Subscribe to Order Book
  - Subscribe to Trades
  - Subscribe to Candlestick Data
- [User Data Stream](./docs/spot/ws-user.md)
  - Account Updates
  - Order Updates

## Futures Trading
### REST API
- [Market Data](./docs/futures/market.md)
  - Get Contracts
  - Get Tickers
  - Get Order Book
  - Get Recent Trades
  - Get Candlestick Data
  - Get Funding Rate
- [Trading](./docs/futures/trading.md)
  - Place Order
  - Cancel Order
  - Get Position
  - Get Order Status
  - Get Open Orders
- [Account](./docs/futures/account.md)
  - Get Account Info
  - Get Position Risk
  - Get Funding History

### WebSocket API
- [Market Data Stream](./docs/futures/ws-market.md)
  - Subscribe to Tickers
  - Subscribe to Order Book
  - Subscribe to Trades
  - Subscribe to Funding Rate
- [User Data Stream](./docs/futures/ws-user.md)
  - Position Updates
  - Order Updates
  - Account Updates

## Options Trading
### REST API
- [Market Data](./docs/options/market.md)
  - Get Contracts
  - Get Tickers
  - Get Order Book
  - Get Recent Trades
- [Trading](./docs/options/trading.md)
  - Place Order
  - Cancel Order
  - Get Position
  - Get Order Status
- [Account](./docs/options/account.md)
  - Get Account Info
  - Get Position Risk

### WebSocket API
- [Market Data Stream](./docs/options/ws-market.md)
  - Subscribe to Tickers
  - Subscribe to Order Book
  - Subscribe to Trades
- [User Data Stream](./docs/options/ws-user.md)
  - Position Updates
  - Order Updates
  - Account Updates

## Unified Trading
### REST API
- [Account](./docs/unified/account.md)
  - Get Unified Account
  - Get Account Balance
  - Get Position Risk
- [Trading](./docs/unified/trading.md)
  - Place Order
  - Cancel Order
  - Get Position
  - Get Order Status

### WebSocket API
- [User Data Stream](./docs/unified/ws-user.md)
  - Account Updates
  - Position Updates
  - Order Updates

## Additional Resources
- [Official API Documentation](https://www.gate.io/docs/developers/apiv4/)
- [API Status](https://status.gate.io/)
- [Gate.io Blog](https://www.gate.io/blog)
- [Telegram Group](https://t.me/gate_io)