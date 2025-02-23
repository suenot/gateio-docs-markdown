# Error Codes

This document lists all error codes that may be returned by the Gate.io API v4, along with their descriptions and recommended handling approaches.

## HTTP Status Codes

### 2XX Success
- 200 OK: Request successful
- 201 Created: Resource created successfully
- 204 No Content: Request successful with no content to return

### 4XX Client Errors
- 400 Bad Request: Invalid request format or parameters
- 401 Unauthorized: Authentication failed or invalid credentials
- 403 Forbidden: Permission denied for the requested operation
- 404 Not Found: Requested resource does not exist
- 429 Too Many Requests: Rate limit exceeded

### 5XX Server Errors
- 500 Internal Server Error: Server encountered an error
- 502 Bad Gateway: Gateway or proxy error
- 503 Service Unavailable: Service temporarily unavailable

## API Error Codes

### Authentication Errors (1XXX)
- 1001: Invalid API key
- 1002: Invalid signature
- 1003: Invalid timestamp
- 1004: IP not allowed
- 1005: Permission denied

### Request Errors (2XXX)
- 2001: Invalid parameter
- 2002: Invalid request format
- 2003: Missing required parameter
- 2004: Invalid currency pair
- 2005: Invalid order type
- 2006: Invalid price
- 2007: Invalid amount
- 2008: Invalid order ID

### Trading Errors (3XXX)
- 3001: Insufficient balance
- 3002: Order amount too small
- 3003: Order price out of range
- 3004: Market order amount exceeds limit
- 3005: Order would trigger immediate liquidation
- 3006: Position would exceed limit

### System Errors (4XXX)
- 4001: System busy
- 4002: Service unavailable
- 4003: Gateway timeout
- 4004: Database error

## Error Response Format

```json
{
    "code": 1001,
    "message": "Invalid API key",
    "label": "INVALID_KEY"
}
```

## Error Handling Best Practices

1. **Implement Proper Error Handling**
   - Always check the response status code
   - Parse the error message and code
   - Log errors appropriately

2. **Handle Rate Limits**
   - Implement exponential backoff
   - Track remaining rate limits
   - Handle 429 errors properly

3. **Retry Strategy**
   - Retry on 5XX errors
   - Avoid retrying on 4XX errors
   - Implement maximum retry attempts

4. **System Status**
   - Monitor system status
   - Subscribe to status updates
   - Have fallback mechanisms

## Common Troubleshooting

1. **Authentication Issues**
   - Verify API key and secret
   - Check system clock synchronization
   - Validate request signing process

2. **Invalid Parameters**
   - Review API documentation
   - Validate input before sending
   - Check parameter formats

3. **Rate Limiting**
   - Monitor rate limit headers
   - Implement request queuing
   - Optimize request frequency

## Next Steps

- Review [Rate Limits](./rate-limits.md)
- Explore product-specific endpoints
- Join our [Telegram Group](https://t.me/gate_io) for support