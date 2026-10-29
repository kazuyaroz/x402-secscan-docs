# x402-secscan v1.0.0 — Security Headers Analyzer

**Endpoint:** `POST https://contract-blair-helmet-postposted.trycloudflare.com/analyze`
**Input:** `{"url": "https://example.com"}`
**Auth:** x402 v2 (EIP-3009 TransferWithAuthorization on Base USDC)
**Price:** 0.01 USDC per request

## Payment Flow
1. POST without payment header → HTTP 402 + `PAYMENT-REQUIRED` offer
2. Sign EIP-712 `TransferWithAuthorization` (EIP-3009)
3. Resend with `PAYMENT-SIGNATURE` header → HTTP 200 + result

## What It Analyzes
- 10 security headers (weighted scoring): CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Permissions-Policy, Referrer-Policy, X-XSS-Protection, COOP, CORP, Cache-Control
- Information disclosure headers (Server, X-Powered-By, etc.)
- Cookie security flags (Secure, HttpOnly, SameSite)

## Output
JSON with: target, http_status, score, max_score, percentage, grade,
headers_present, headers_missing, info_leaks, cookie_issues, findings[]

## Grading
A+ (90%+), A (80%+), B (70%+), C (60%+), D (50%+), E (30%+), F (<30%)
