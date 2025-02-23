# Gate.io Options WebSocket API v4

## Overview
The Gate.io Options WebSocket API provides real-time market data and trading functionality for options trading. This document describes the available WebSocket endpoints and their usage.

## Connection
- WebSocket URL: `wss://ws.gate.io/v4/options`
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

### Private Channels
- Orders
- Positions
- Account Updates

## Message Format

### Subscribe Message
```json
{
  "time": 1234567890123,
  "channel": "spot.tickers",
  "event": "subscribe",
  "payload": ["BTC_USDT"]
}
```

### Unsubscribe Message
```json
{
  "time": 1234567890123,
  "channel": "spot.tickers",
  "event": "unsubscribe",
  "payload": ["BTC_USDT"]
}
```

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