# API Reference

This page provides documentation for the Risk Data Center APIs.

## REST API

### Authentication

```
POST /api/auth
```

**Request Body:**

```json
{
  "username": "your_username",
  "password": "your_password"
}
```

**Response:**

```json
{
  "token": "your_jwt_token",
  "expires_at": "2025-05-24T00:00:00Z"
}
```

### Data Endpoints

#### Get Risk Data

```
GET /api/risk-data
```

**Query Parameters:**

| Parameter | Type   | Description                     |
|-----------|--------|---------------------------------|
| date      | string | Date in format YYYY-MM-DD       |
| category  | string | Risk category (market, credit)  |
| limit     | number | Maximum number of results       |

**Response:**

```json
{
  "results": [
    {
      "id": "123",
      "date": "2025-05-23",
      "category": "market",
      "value": 123.45,
      "metrics": {
        "var": 12.34,
        "cvar": 15.67
      }
    }
  ],
  "total": 1
}
```

## Python Client Library

```python
from risk_datacenter import RiskClient

# Initialize client
client = RiskClient(api_key="your_api_key")

# Get risk data
data = client.get_risk_data(
    date="2025-05-23",
    category="market",
    limit=10
)

# Print results
for item in data.results:
    print(f"ID: {item.id}, Value: {item.value}")
```

## Example Code

```python
# Example: Calculate VaR for a portfolio
import numpy as np
from risk_datacenter import RiskClient

def calculate_var(portfolio_data, confidence_level=0.95):
    returns = np.array([d.return_value for d in portfolio_data])
    var = np.percentile(returns, (1 - confidence_level) * 100)
    return abs(var)

# Get portfolio data
client = RiskClient(api_key="your_api_key")
portfolio_data = client.get_portfolio_returns(
    portfolio_id="main_portfolio",
    lookback_days=252
)

# Calculate 95% VaR
var_95 = calculate_var(portfolio_data, confidence_level=0.95)
print(f"95% VaR: {var_95:.2f}%")
```

## Error Codes

| Code | Description                |
|------|----------------------------|
| 400  | Bad Request                |
| 401  | Unauthorized               |
| 403  | Forbidden                  |
| 404  | Not Found                  |
| 429  | Too Many Requests          |
| 500  | Internal Server Error      |
| 503  | Service Unavailable        |
