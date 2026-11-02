# x402-secscan — Security Headers Analyzer API

A paid HTTP API that scores a target's security headers. Built as an **x402 v2** pay-per-request service: the caller signs a USDC `TransferWithAuthorization` on Base, and the analysis comes back as JSON.

**Price:** 0.01 USDC per request · **Auth:** x402 v2 (EIP-3009) · **Chain:** Base

---

## Endpoint

```
POST /analyze
Content-Type: application/json

{"url": "https://example.com"}
```

## Payment flow

1. `POST /analyze` with no payment header → **HTTP 402** + a `PAYMENT-REQUIRED` offer describing price, asset and payee.
2. Sign an EIP-712 `TransferWithAuthorization` (EIP-3009) for the offered amount.
3. Resend with a `PAYMENT-SIGNATURE` header → **HTTP 200** + the analysis JSON.

No API key, no account, no subscription — the payment *is* the authorization.

## What it analyzes

**10 weighted security headers**

CSP · HSTS · X-Frame-Options · X-Content-Type-Options · Permissions-Policy · Referrer-Policy · X-XSS-Protection · COOP · CORP · Cache-Control

**Plus**

- Information-disclosure headers (`Server`, `X-Powered-By`, …)
- Cookie security flags (`Secure`, `HttpOnly`, `SameSite`)

## Output

```json
{
  "target": "https://example.com",
  "http_status": 200,
  "score": 74,
  "max_score": 100,
  "percentage": 74,
  "grade": "B",
  "headers_present": ["...", "..."],
  "headers_missing": ["...", "..."],
  "info_disclosure": ["..."],
  "cookies": ["..."]
}
```

## Docs

- [`guide.md`](guide.md) — full request/response reference and the payment handshake
- [`terms.md`](terms.md) — service terms

---

*Part of my security research work — see the [portfolio](https://kazuyaroz.github.io/portfolio).*
