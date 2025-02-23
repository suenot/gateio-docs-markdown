# Rate Limits

Gate.io API v4 implements rate limiting to ensure fair usage and system stability. This document explains the rate limiting policies and how to handle them effectively.

## Rate Limit Types

### Public Endpoints
- Market data: 900 requests per minute per IP
- Trading pairs info: 600 requests per minute per IP
- Order book: 300 requests per minute per IP

### Private Endpoints
- Trading operations: 300 requests per minute per API key
- Account operations: 180 requests per minute per API key
- Withdrawal operations: 60 requests per minute per API key

### WebSocket Connections
- Maximum 50 connections per IP
- Maximum 20 subscriptions per connection

## Rate Limit Headers

Each API response includes headers with rate limit information:
```
X-RateLimit-Remaining: Number of requests remaining
X-RateLimit-Limit: Total requests allowed
X-RateLimit-Reset: Timestamp when the limit resets
```

## Best Practices

1. **Monitor Rate Limits**
   - Track remaining requests using response headers
   - Implement rate tracking in your application
   - Plan requests to stay within limits

2. **Handle Rate Limit Errors**
   - Watch for 429 HTTP status code
   - Implement exponential backoff
   - Queue requests when approaching limits

3. **Optimize Request Usage**
   - Batch operations when possible
   - Cache frequently accessed data
   - Use WebSocket for real-time updates

## Implementation Examples

### Python Rate Limit Handler
```python
import time
from datetime import datetime

class RateLimitHandler:
    def __init__(self):
        self.remaining = None
        self.limit = None
        self.reset_time = None

    def update_limits(self, headers):
        self.remaining = int(headers.get('X-RateLimit-Remaining', 0))
        self.limit = int(headers.get('X-RateLimit-Limit', 0))
        self.reset_time = int(headers.get('X-RateLimit-Reset', 0))

    def should_retry(self):
        if self.remaining is None or self.remaining > 0:
            return False
        
        now = time.time()
        if now >= self.reset_time:
            return False
            
        return True

    def wait_time(self):
        if not self.should_retry():
            return 0
        return max(0, self.reset_time - time.time())
```

### Node.js Rate Limit Handler
```javascript
class RateLimitHandler {
    constructor() {
        this.remaining = null;
        this.limit = null;
        this.resetTime = null;
    }

    updateLimits(headers) {
        this.remaining = parseInt(headers['x-ratelimit-remaining']) || 0;
        this.limit = parseInt(headers['x-ratelimit-limit']) || 0;
        this.resetTime = parseInt(headers['x-ratelimit-reset']) || 0;
    }

    shouldRetry() {
        if (this.remaining === null || this.remaining > 0) {
            return false;
        }

        const now = Math.floor(Date.now() / 1000);
        if (now >= this.resetTime) {
            return false;
        }

        return true;
    }

    getWaitTime() {
        if (!this.shouldRetry()) {
            return 0;
        }
        return Math.max(0, this.resetTime - Math.floor(Date.now() / 1000));
    }
}
```

## Common Issues

1. **Unexpected Rate Limits**
   - Check for IP-based vs API key-based limits
   - Verify rate limit scope (endpoint specific)
   - Monitor multiple application instances

2. **Rate Limit Exceeded**
   - Implement proper backoff strategy
   - Consider using multiple API keys
   - Optimize request patterns

## WebSocket Considerations

1. **Connection Limits**
   - Monitor active connections
   - Implement connection pooling
   - Handle reconnection properly

2. **Subscription Limits**
   - Group related subscriptions
   - Implement subscription management
   - Handle subscription errors

## Next Steps

- Explore [API Endpoints](./readme.md)
- Review [Error Codes](./error-codes.md)
- Join our [Telegram Group](https://t.me/gate_io) for support