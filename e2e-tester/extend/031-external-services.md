## Larixon External Service Mocking

### Playwright network mocking

For browser-based E2E tests, use `page.route()` to intercept external service calls at the network level:

```python
async def mock_payment(route):
    await route.fulfill(json={"status": "success", "transaction_id": "test-123"})

await page.route("**/api/v1/payments/**", mock_payment)
```

### Backend-level mocking

For API-level E2E flows, mock external services at the Python level using `unittest.mock.patch` or `responses`:

```python
@patch("apps.payments.client.PaymentGateway.charge")
def test_purchase_flow(self, mock_charge):
    mock_charge.return_value = {"status": "success"}
    # ... multi-step flow
```

### Larixon-specific external services

Mock these when they appear in E2E flows:
- Payment processing (market-specific gateways)
- Email/SMS notifications
- eMongolia authentication (mn market)
- GDPR consent services (bz market)

### When to use which approach

| Approach | When to use |
|----------|------------|
| `page.route()` | Playwright browser tests — intercept at network level |
| `unittest.mock.patch` | API-level E2E tests — mock at Python level |
| `responses` library | When testing code that uses `requests` library directly |
