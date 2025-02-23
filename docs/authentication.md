# Authentication

Gate.io API v4 uses HMAC SHA512 authentication to ensure secure access to private endpoints. This guide explains how to authenticate your API requests.

## API Keys

To access private endpoints, you need:
- **API Key**: Your public identifier
- **API Secret**: Your private key for signing requests

### Key Permissions

When creating API keys, you can set different permission levels:
- Read-only: For market data and account information
- Trade: For placing and managing orders
- Withdraw: For withdrawal operations

## Authentication Process

### Request Signing

1. **Create a timestamp**
   - Use current Unix timestamp in seconds

2. **Prepare the payload string**
   ```
   METHOD|PATH|QUERY_STRING|HASHED_PAYLOAD|TIMESTAMP
   ```
   - METHOD: HTTP method (GET, POST, etc.)
   - PATH: Request path without host and query string
   - QUERY_STRING: URL parameters sorted alphabetically
   - HASHED_PAYLOAD: SHA512 hash of the request body (empty string for GET)
   - TIMESTAMP: The timestamp from step 1

3. **Generate the signature**
   - Create HMAC SHA512 hash of the payload using your API Secret
   - Encode the result in hexadecimal

### Request Headers

Include these headers in your request:
```
KEY: Your API Key
Timestamp: Your timestamp
SIGN: Your generated signature
```

## Example Implementation

### Python
```python
import time
import hmac
import hashlib

def sign_request(method, path, query_string, body, api_key, api_secret):
    t = str(int(time.time()))
    m = hashlib.sha512()
    m.update(body.encode('utf-8'))
    hashed_payload = m.hexdigest()
    
    s = '|'.join([method, path, query_string, hashed_payload, t])
    
    sign = hmac.new(
        api_secret.encode('utf-8'),
        s.encode('utf-8'),
        hashlib.sha512
    ).hexdigest()
    
    return {
        'KEY': api_key,
        'Timestamp': t,
        'SIGN': sign
    }
```

### Node.js
```javascript
const crypto = require('crypto');

function signRequest(method, path, queryString, body, apiKey, apiSecret) {
    const timestamp = Math.floor(Date.now() / 1000).toString();
    const hashedPayload = crypto
        .createHash('sha512')
        .update(body)
        .digest('hex');
    
    const signString = [
        method,
        path,
        queryString,
        hashedPayload,
        timestamp
    ].join('|');
    
    const signature = crypto
        .createHmac('sha512', apiSecret)
        .update(signString)
        .digest('hex');
    
    return {
        KEY: apiKey,
        Timestamp: timestamp,
        SIGN: signature
    };
}
```

## Security Best Practices

1. **Protect Your API Secret**
   - Never share your API Secret
   - Store it securely
   - Use environment variables

2. **Key Management**
   - Regularly rotate your API keys
   - Use different keys for different applications
   - Limit IP addresses when possible

3. **Request Validation**
   - Verify SSL/TLS certificates
   - Check server timestamps
   - Implement request timeout

## Common Issues

1. **Invalid Signature**
   - Check timestamp accuracy
   - Verify payload string format
   - Ensure correct API Secret usage

2. **Request Timeout**
   - Verify system clock synchronization
   - Check network connectivity
   - Implement retry logic

## Next Steps

- Review [Error Codes](./error-codes.md)
- Understand [Rate Limits](./rate-limits.md)
- Explore API endpoints in product-specific sections