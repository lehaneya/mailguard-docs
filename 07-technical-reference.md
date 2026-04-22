# Technical Reference for Administrators

**Version:** 1.0  
**Last Updated:** April 11, 2026  
**For:** System administrators, developers, API integrators

---

## API Overview

MailGuard provides REST APIs for integrating domain monitoring into your workflows. All endpoints require JWT authentication.

### Base URL

```
https://api.mailguard.alsecurity.com/api
```

### Authentication

Include a Bearer token in the `Authorization` header:

```
Authorization: Bearer <your_jwt_token>
```

Tokens are valid for 15 minutes. Refresh tokens last 7 days.

---

## Authentication API

### Register

```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "strong-password"
}
```

**Response:**
```json
{
  "data": {
    "id": "uuid-here",
    "email": "user@example.com"
  }
}
```

---

### Login

```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password"
}
```

**Response:**
```json
{
  "data": {
    "accessToken": "jwt-token",
    "refreshToken": "refresh-jwt-token"
  }
}
```

Store both tokens locally. Use `accessToken` for API calls. Use `refreshToken` to get a new `accessToken` when expired.

---

### Refresh Token

```
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "refresh-jwt-token"
}
```

**Response:**
```json
{
  "data": {
    "accessToken": "new-jwt-token",
    "refreshToken": "new-refresh-jwt-token"
  }
}
```

---

## Domain API

### List Domains

```
GET /domains
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": [
    {
      "id": "uuid-1",
      "domainName": "example.com",
      "verified": true,
      "createdAt": "2026-04-01T10:00:00Z",
      "verificationToken": "v1_abc123..."
    }
  ]
}
```

---

### Get Domain

```
GET /domains/{domainId}
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "id": "uuid-1",
    "domainName": "example.com",
    "verified": true,
    "verificationToken": "v1_abc123...",
    "createdAt": "2026-04-01T10:00:00Z"
  }
}
```

---

### Add Domain

```
POST /domains
Authorization: Bearer <token>
Content-Type: application/json

{
  "domainName": "example.com"
}
```

**Response:**
```json
{
  "data": {
    "id": "uuid-1",
    "domainName": "example.com",
    "verified": false,
    "verificationToken": "v1_abc123_verification_string"
  }
}
```

Return the `verificationToken` to your user so they can add the TXT record to their DNS.

---

### Verify Domain

```
POST /domains/{domainId}/verify
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "id": "uuid-1",
    "domainName": "example.com",
    "verified": true
  }
}
```

This polls MailGuard's DNS to check if the verification TXT record exists.

---

### Delete Domain

```
DELETE /domains/{domainId}
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": null
}
```

---

## DNS Check Results API

### Get Domain Score

```
GET /domains/{domainId}/score
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "domainId": "uuid-1",
    "numericScore": 78,
    "letterGrade": "C",
    "lastCheckedAt": "2026-04-11T14:00:00Z"
  }
}
```

---

### Get All Check Results

```
GET /domains/{domainId}/checks
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": [
    {
      "checkType": "SPF",
      "status": "PASS",
      "lastCheckedAt": "2026-04-11T14:00:00Z",
      "details": {
        "record": "v=spf1 include:_spf.google.com ~all",
        "issueCount": 0
      }
    },
    {
      "checkType": "DKIM",
      "status": "WARNING",
      "lastCheckedAt": "2026-04-11T14:00:00Z",
      "details": {
        "issueCount": 1,
        "issues": [
          {
            "severity": "WARNING",
            "message": "Default DKIM selector not found; using custom selector."
          }
        ]
      }
    },
    {
      "checkType": "DMARC",
      "status": "FAIL",
      "lastCheckedAt": "2026-04-11T14:00:00Z",
      "details": {
        "record": null,
        "issueCount": 1,
        "issues": [
          {
            "severity": "CRITICAL",
            "message": "DMARC record not found. Add a DMARC policy to prevent spoofing."
          }
        ]
      }
    }
  ]
}
```

### Check Types

Possible values for `checkType`:

- `SPF`
- `DKIM`
- `DMARC`
- `MX`
- `BIMI`
- `BLACKLIST`
- `MTA_STS`
- `DANE`
- `TLS_RPT`
- `PTR`
- `CERT_EXPIRY`
- `SUBDOMAIN`

### Check Statuses

- `PASS` — Check passed
- `WARNING` — Check passed but has minor issues
- `FAIL` — Check failed

---

### Run Checks Now

```
POST /domains/{domainId}/checks/run
Authorization: Bearer <token>
```

Forces an immediate re-check of all DNS records (otherwise checks run on a schedule).

**Response:**
```json
{
  "data": [
    // ... check results (same format as above)
  ]
}
```

---

## Alerts API

### Get Alerts

```
GET /alerts?page=0&size=20
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "content": [
      {
        "id": "alert-uuid",
        "domainId": "domain-uuid",
        "domainName": "example.com",
        "checkType": "SPF",
        "severity": "CRITICAL",
        "message": "SPF record is missing.",
        "acknowledged": false,
        "createdAt": "2026-04-11T14:00:00Z"
      }
    ],
    "pageNumber": 0,
    "pageSize": 20,
    "totalElements": 42,
    "totalPages": 3
  }
}
```

---

### Get Unacknowledged Alerts

```
GET /alerts?unacknowledged=true&page=0&size=20
Authorization: Bearer <token>
```

Same response format as above, but filtered to only unacknowledged alerts.

---

### Acknowledge Alert

```
POST /alerts/{alertId}/acknowledge
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "id": "alert-uuid",
    "acknowledged": true,
    "acknowledgedAt": "2026-04-11T14:05:00Z"
  }
}
```

---

### Get Alert Preferences

```
GET /alerts/preferences
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": [
    {
      "id": "pref-uuid",
      "checkType": "SPF",
      "domain": "example.com",
      "enabled": true,
      "frequency": "IMMEDIATELY"
    }
  ]
}
```

---

### Create Alert Preference

```
POST /alerts/preferences
Authorization: Bearer <token>
Content-Type: application/json

{
  "checkType": "DMARC",
  "domainId": "domain-uuid",
  "enabled": true,
  "frequency": "IMMEDIATELY"
}
```

---

## Premium API (AI & DMARC)

### Get DMARC RUA Address

```
GET /premium/dmarc/{domainId}/rua-address
Authorization: Bearer <token>
```

Requires Premium subscription.

**Response:**
```json
{
  "data": {
    "ruaAddress": "rua-abc123def@ingest.mailguard.alsecurity.com",
    "dmarcRecord": "v=DMARC1; p=none; rua=mailto:rua-abc123def@ingest.mailguard.alsecurity.com"
  }
}
```

---

### Upload DMARC Report

```
POST /premium/dmarc/{domainId}/upload
Authorization: Bearer <token>
Content-Type: multipart/form-data

file: <xml or gzip file>
```

Requires Premium subscription.

**Response:**
```json
{
  "data": {
    "id": "report-uuid",
    "domainId": "domain-uuid",
    "orgName": "Google",
    "reportId": "1234567890",
    "recordCount": 150,
    "receivedAt": "2026-04-11T02:00:00Z"
  }
}
```

---

### Get DMARC Summary

```
GET /premium/dmarc/{domainId}/summary
Authorization: Bearer <token>
```

Requires Premium subscription.

**Response:**
```json
{
  "data": {
    "totalReports": 7,
    "totalMessages": 350000,
    "passingMessages": 315000,
    "passRate": 90
  }
}
```

---

### Get DMARC AI Analysis

```
GET /premium/dmarc/{domainId}/ai-analysis?refresh=false
Authorization: Bearer <token>
```

Requires Premium subscription.

**Query Parameters:**
- `refresh=true` — Force re-analysis (ignores cache)
- `refresh=false` (default) — Use cached result if available

**Response:**
```json
{
  "data": {
    "id": "analysis-uuid",
    "domainId": "domain-uuid",
    "generatedAt": "2026-04-11T14:00:00Z",
    "expiresAt": "2026-05-11T14:00:00Z",
    "fromCache": false,
    "modelUsed": "claude-3-5-sonnet",
    "narrative": "Your domain has solid email authentication...",
    "riskExplanation": "The 5% failure rate indicates...",
    "remediationSteps": "1. Review failing senders...\n2. Update SPF...",
    "summary": "Overall security posture is good."
  }
}
```

---

## Billing API

### Get Subscription

```
GET /billing/subscription
Authorization: Bearer <token>
```

**Response:**
```json
{
  "data": {
    "tier": "PREMIUM",
    "status": "ACTIVE",
    "includedDomains": 1,
    "additionalDomains": 2,
    "currentPeriodStart": "2026-04-01T00:00:00Z",
    "currentPeriodEnd": "2026-05-01T00:00:00Z",
    "trialEnd": null,
    "cancelAtPeriodEnd": false
  }
}
```

### Tiers

- `STANDARD`
- `PREMIUM`

### Statuses

- `TRIALING` — In free trial
- `ACTIVE` — Active subscription
- `PAST_DUE` — Payment failed
- `CANCELED` — Subscription canceled
- `INCOMPLETE` — Incomplete signup

---

## Error Handling

All error responses follow this format:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Domain name is required"
  }
}
```

### Common Error Codes

| Code | HTTP | Meaning |
|------|------|---------|
| `INVALID_CREDENTIALS` | 401 | Wrong email/password |
| `UNAUTHORIZED` | 401 | Token invalid or expired |
| `FORBIDDEN` | 403 | Not allowed (e.g., not Premium) |
| `NOT_FOUND` | 404 | Resource doesn't exist |
| `VALIDATION_ERROR` | 400 | Invalid input |
| `DOMAIN_ALREADY_EXISTS` | 409 | Domain already registered |
| `SUBSCRIPTION_REQUIRED` | 402 | Feature requires paid subscription |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `INTERNAL_SERVER_ERROR` | 500 | Server error |

---

## Rate Limits

- **General endpoints:** 100 requests per minute
- **AI analysis:** 1 per domain per day
- **Check runner:** 1 per domain per minute

Exceeding limits returns `429 Too Many Requests`.

---

## Webhooks (Coming Soon)

Webhook support is planned for future releases. Subscribe to updates at mailguard.alsecurity.com/api.

---

## SDK & Client Libraries

Official SDKs:

- **JavaScript/TypeScript:** `npm install @mailguard/sdk`
- **Python:** `pip install mailguard`
- **Go:** `go get github.com/mailguard/go-sdk`

---

## Code Examples

### JavaScript/TypeScript

```typescript
import axios from 'axios';

const API_BASE = 'https://api.mailguard.alsecurity.com/api';

// Login
const loginRes = await axios.post(`${API_BASE}/auth/login`, {
  email: 'user@example.com',
  password: 'password'
});

const token = loginRes.data.data.accessToken;

// Get domains
const domainsRes = await axios.get(`${API_BASE}/domains`, {
  headers: { Authorization: `Bearer ${token}` }
});

const domains = domainsRes.data.data;

// Get check results
const checksRes = await axios.get(
  `${API_BASE}/domains/${domains[0].id}/checks`,
  { headers: { Authorization: `Bearer ${token}` } }
);

console.log(checksRes.data.data);
```

---

### Python

```python
import requests

API_BASE = 'https://api.mailguard.alsecurity.com/api'

# Login
login_res = requests.post(
    f'{API_BASE}/auth/login',
    json={'email': 'user@example.com', 'password': 'password'}
)
token = login_res.json()['data']['accessToken']

# Get domains
headers = {'Authorization': f'Bearer {token}'}
domains_res = requests.get(f'{API_BASE}/domains', headers=headers)
domains = domains_res.json()['data']

# Get check results
checks_res = requests.get(
    f'{API_BASE}/domains/{domains[0]["id"]}/checks',
    headers=headers
)
print(checks_res.json()['data'])
```

---

### cURL

```bash
# Login
TOKEN=$(curl -s -X POST https://api.mailguard.alsecurity.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}' \
  | jq -r '.data.accessToken')

# Get domains
curl -s -X GET https://api.mailguard.alsecurity.com/api/domains \
  -H "Authorization: Bearer $TOKEN" \
  | jq .
```

---

## Configuration for Self-Hosted Deployments

MailGuard can be deployed on-premise (ask about enterprise licensing).

### Environment Variables

```bash
# Database
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/mailguard
SPRING_DATASOURCE_USERNAME=mailguard
SPRING_DATASOURCE_PASSWORD=secure-password

# JWT
JWT_SECRET=your-secret-key-must-be-at-least-256-bits-long

# Frontend
FRONTEND_URL=https://mailguard.example.com

# SendGrid (for alerts)
SENDGRID_API_KEY=your-sendgrid-key
SENDGRID_FROM_EMAIL=alerts@example.com

# Stripe (for billing)
STRIPE_SECRET_KEY=your-stripe-secret
STRIPE_WEBHOOK_SECRET=your-webhook-secret
STRIPE_PRICE_STANDARD=price_xxxx
STRIPE_PRICE_PREMIUM=price_yyyy
STRIPE_PRICE_ADDITIONAL_DOMAIN=price_zzzz

# AI Provider (Claude API)
ANTHROPIC_API_KEY=your-anthropic-key

# DNS
DNS_RESOLVERS=8.8.8.8,8.8.4.4,1.1.1.1
DNS_TIMEOUT_SECONDS=5
DNS_MAX_RETRIES=3
```

---

## Support for Developers

- **API Docs:** api-docs.mailguard.alsecurity.com
- **GitHub:** github.com/mailguard (contact us for access)
- **Slack:** developer-slack.mailguard.alsecurity.com
- **Email:** developers@mailguard.alsecurity.com

---

## Next Steps

1. **Get API credentials** — Log in and generate an API token in Settings
2. **Read the API docs** — Full endpoint reference at api-docs.mailguard.alsecurity.com
3. **Try the SDK** — Use our TypeScript, Python, or Go SDK
4. **Build integrations** — Connect MailGuard to your workflow
5. **Contact support** if you hit issues

Happy integrating! 🚀
