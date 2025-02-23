# Gate.io Perpetual Futures WebSocket API v4

## Overview
The Gate.io Perpetual Futures WebSocket API provides real-time market data and trading functionality for perpetual futures contracts. This document outlines the available WebSocket endpoints and their usage.

## Connection
- WebSocket URL: `wss://ws.gate.io/v4/futures/usdt`
- All messages are in JSON format
- All timestamps are in milliseconds

## Authentication
To access private data and trading endpoints, you need to authenticate your WebSocket connection:

1. Generate an API key and secret from your Gate.io account
2. Sign your connection request using your API credentials
3. Send the authentication message after establishing the connection

## Available Channels

### Public Channels
- Tickers
- Order Book
- Trades
- Candlesticks
- Index Price
- Mark Price
- Funding Rate

### Private Channels
- Orders
- Positions
- Account Updates
- Auto-Deleveraging
- Liquidation Orders

## Message Format

### Subscribe Message
```json
{
  "time": 1234567890123,
  "channel": "futures.tickers",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Unsubscribe Message
```json
{
  "time": 1234567890123,
  "channel": "futures.tickers",
  "event": "unsubscribe",
  "payload": ["BTC_USDT"]
}
```

## Channel Details

### Tickers Channel
- Provides 24h market statistics
- Updates on every trade
- Includes last price, volume, and price changes
- Funding rate information

### Order Book Channel
- Provides real-time order book data
- Supports different levels (5, 10, 20, 50)
- Includes price and quantity for each level

### Trades Channel
- Real-time trade execution data
- Includes price, quantity, and side
- Timestamp for each trade

### Funding Rate Channel
- Real-time funding rate updates
- Predicted funding rate
- Next funding time

## Error Handling
- Connection errors
- Authentication errors
- Subscription errors
- Rate limiting

## Best Practices
1. Implement heartbeat mechanism
2. Handle reconnection scenarios
3. Process messages asynchronously
4. Implement error handling
5. Follow rate limits
6. Maintain order book state properly
7. Monitor funding rates regularly