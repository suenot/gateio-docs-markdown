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

1. Create a string to sign:
   ```
   HTTPMethod|Path|Query|Timestamp|Payload
   ```
2. Generate HMAC SHA512 signature using your API Secret
3. Add required headers:
   - `KEY`: Your API Key
   - `Timestamp`: Current Unix timestamp
   - `SIGN`: Generated signature

## Code Examples

### Python

```python
import time
import hmac
import hashlib
import requests
from gate_api import ApiClient, Configuration

def sign_request(method: str, path: str, query_string: str = '', body: str = ''):
    t = time.time()
    m = hashlib.sha512()
    m.update((method + '|' + path + '|' + query_string + '|' + 
             str(t) + '|' + body).encode('utf-8'))
    sign = hmac.new(api_secret.encode('utf-8'), 
                    m.digest(), 
                    hashlib.sha512).hexdigest()
    return {
        'KEY': api_key,
        'Timestamp': str(t),
        'SIGN': sign
    }

# Usage example
config = Configuration(key='your_api_key', secret='your_api_secret')
client = ApiClient(config)
```

### Node.js

```javascript
const crypto = require('crypto');
const GateApi = require('@gateio/gate-api');

function signRequest(method, path, queryString = '', body = '') {
    const timestamp = Math.floor(Date.now() / 1000);
    const stringToSign = `${method}|${path}|${queryString}|${timestamp}|${body}`;
    
    const hash = crypto.createHash('sha512')
                      .update(stringToSign)
                      .digest();
    
    const signature = crypto.createHmac('sha512', apiSecret)
                          .update(hash)
                          .digest('hex');

    return {
        'KEY': apiKey,
        'Timestamp': timestamp.toString(),
        'SIGN': signature
    };
}

// Usage example
const client = new GateApi.ApiClient();
client.setApiKeySecret('your_api_key', 'your_api_secret');
```

### Go

```go
package main

import (
    "crypto/hmac"
    "crypto/sha512"
    "encoding/hex"
    "fmt"
    "strconv"
    "time"
    "github.com/gateio/gateapi-go/v6"
)

func signRequest(method, path, queryString, body string) map[string]string {
    timestamp := strconv.FormatInt(time.Now().Unix(), 10)
    stringToSign := fmt.Sprintf("%s|%s|%s|%s|%s", 
        method, path, queryString, timestamp, body)

    h := sha512.New()
    h.Write([]byte(stringToSign))
    
    mac := hmac.New(sha512.New, []byte(apiSecret))
    mac.Write(h.Sum(nil))
    sign := hex.EncodeToString(mac.Sum(nil))

    return map[string]string{
        "KEY":       apiKey,
        "Timestamp": timestamp,
        "SIGN":      sign,
    }
}

// Usage example
client := gateapi.NewAPIClient(gateapi.NewConfiguration())
client.SetApiKeySecret("your_api_key", "your_api_secret")
```

## Best Practices

1. **Never share your API Secret**
   - Keep your API Secret secure
   - Don't include it in client-side code
   - Don't commit it to version control

2. **Use appropriate permissions**
   - Only enable the permissions you need
   - Use read-only keys when possible
   - Create separate keys for different applications

3. **Handle timestamps correctly**
   - Use Unix timestamp in seconds
   - Ensure your system clock is synchronized
   - Server allows ±30 seconds time difference

4. **Implement rate limiting**
   - Track your API usage
   - Implement exponential backoff
   - Handle rate limit errors gracefully

5. **Security recommendations**
   - Use HTTPS only
   - Implement IP whitelisting
   - Rotate API keys periodically
   - Monitor for unauthorized access