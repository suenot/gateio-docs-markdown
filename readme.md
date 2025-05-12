# Gate.io API v4 Documentation
[![Context7 Docs](https://badgen.net/badge/Context7/GateIO%20Docs/blue)](https://context7.com/suenot/gateio-docs-markdown)

## Getting Started
- [Introduction](./docs/introduction.md)
- [Authentication](./docs/authentication.md)
- [Error Codes](./docs/error-codes.md)
- [Rate Limits](./docs/rate-limits.md)

## SDK Integration
- [SDK Examples](./docs/sdk-examples.md)
- [Python SDK](https://github.com/gateio/gateapi-python)
- [Node.js SDK](https://github.com/gateio/gateapi-nodejs)
- [Go SDK](https://github.com/gateio/gateapi-go)

## API Documentation

### Spot Trading
#### REST API
- [Market Data](./docs/spot/market.md)
  - Currency Pairs
  - Tickers
  - Order Book
  - Recent Trades
  - Candlestick Data
- [Trading](./docs/spot/trading.md)
  - Place Order
  - Cancel Order
  - Order Status
  - Open Orders
  - Order History
- [Account](./docs/spot/account.md)
  - Account Balance
  - Account History
- [Margin Trading](./docs/spot/margin.md)
  - Margin Account
  - Borrow
  - Repay
  - Transfer

#### WebSocket API
- [Market Data Stream](./docs/spot/ws-market.md)
  - Tickers
  - Order Book
  - Trades
  - Candlestick Data
- [User Data Stream](./docs/spot/ws-user.md)
  - Account Updates
  - Order Updates

### Futures Trading
#### REST API
- [Market Data](./docs/futures/market.md)
  - Contracts
  - Tickers
  - Order Book
  - Recent Trades
  - Candlestick Data
  - Funding Rate
  - Index Price
- [Trading](./docs/futures/trading.md)
  - Place Order
  - Cancel Order
  - Position Management
  - Order Status
  - Open Orders
  - Stop Orders
  - Reduce-Only Orders
- [Account](./docs/futures/account.md)
  - Account Info
  - Position Risk
  - Funding History
  - Insurance Fund
  - Risk Management

#### WebSocket API
- [Market Data Stream](./docs/futures/ws-market.md)
  - Tickers
  - Order Book
  - Trades
  - Funding Rate
  - Index Price
- [User Data Stream](./docs/futures/ws-user.md)
  - Position Updates
  - Order Updates
  - Account Updates

### Options Trading
#### REST API
- [Market Data](./docs/options/market.md)
  - Contracts
  - Tickers
  - Order Book
  - Recent Trades
  - Greeks
- [Trading](./docs/options/trading.md)
  - Place Order
  - Cancel Order
  - Position Management
  - Order Status
  - Exercise Options
- [Account](./docs/options/account.md)
  - Account Info
  - Position Risk
  - Portfolio Metrics

#### WebSocket API
- [Market Data Stream](./docs/options/ws-market.md)
  - Tickers
  - Order Book
  - Trades
  - Greeks
- [User Data Stream](./docs/options/ws-user.md)
  - Position Updates
  - Order Updates
  - Account Updates
  - Portfolio Updates

### Unified Trading
#### REST API
- [Account](./docs/unified/account.md)
  - Unified Account Info
  - Account Balance
  - Position Risk
  - Portfolio Management
- [Trading](./docs/unified/trading.md)
  - Place Order
  - Cancel Order
  - Position Management
  - Order Status
  - Cross-Product Orders

#### WebSocket API
- [User Data Stream](./docs/unified/ws-user.md)
  - Account Updates
  - Position Updates
  - Order Updates
  - Risk Updates

## Additional Resources
- [Official API Documentation](https://www.gate.io/docs/developers/apiv4/)
- [API Status](https://status.gate.io/)
- [Gate.io Blog](https://www.gate.io/blog)
- [Telegram Group](https://t.me/gate_io)
