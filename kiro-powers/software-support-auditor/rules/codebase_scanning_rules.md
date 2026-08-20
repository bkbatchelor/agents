# Codebase Scanning Rules & Support Tier Classification Matrix

This document provides the reference search patterns, regexes, and tier assignment rules used by the `software-support-auditor` power.

---

## 1. Codebase Error Discovery Search Patterns

When scanning a codebase, search across source files (`.ts`, `.js`, `.py`, `.java`, `.go`, `.cs`, `.rb`, `.rs`, `.php`), API specifications (OpenAPI/Swagger), and configuration files using these patterns:

### A. HTTP Status & API Error Patterns
- `400` / `Bad Request` / `INVALID_INPUT` / `VALIDATION_FAILED`
- `401` / `Unauthorized` / `UNAUTHENTICATED` / `INVALID_CREDENTIALS`
- `403` / `Forbidden` / `PERMISSION_DENIED` / `ACCESS_FORBIDDEN`
- `404` / `Not Found` / `RESOURCE_NOT_FOUND`
- `409` / `Conflict` / `DUPLICATE_ENTRY` / `STATE_CONFLICT`
- `422` / `Unprocessable Entity`
- `500` / `Internal Server Error` / `UNHANDLED_EXCEPTION`
- `502` / `Bad Gateway` / `UPSTREAM_ERROR`
- `503` / `Service Unavailable` / `MAINTENANCE_MODE`

### B. Throw / Catch / Logging Sites
- `throw new [A-Za-z0-9_]+Error`
- `throw new [A-Za-z0-9_]+Exception`
- `raise [A-Za-z0-9_]+Error`
- `panic!\(`
- `logger.error\(` / `log.error\(` / `console.error\(`
- `catch \([A-Za-z0-9_]+ e\)`

### C. Custom Exception & Enum Definitions
- `class [A-Za-z0-9_]+Exception`
- `class [A-Za-z0-9_]+Error`
- `struct [A-Za-z0-9_]+Error`
- `enum ErrorCode` / `enum AppError`
- `const ERR_[A-Z0-9_]+` / `const ERROR_[A-Z0-9_]+`

### D. Infrastructure & Database Boundary Handlers
- `DB_DEADLOCK` / `Lock Wait Timeout` / `QueryTimeoutException`
- `ConnectionPoolExhausted` / `ECONNREFUSED` / `ETIMEDOUT`
- `AWS_S3_ACCESS_DENIED` / `DYNAMODB_THROTTLED` / `S3BucketNotFound`
- `VendorSDKException` / `ThirdPartyApiError`

---

## 2. Complete Support Tier Assignment Matrix (L0 – L4)

| Support Tier | Target Scope & Error Characteristics | Codebase Error Patterns & Examples | Action & Escalation Routing |
| :--- | :--- | :--- | :--- |
| **Level 0 (L0 / AI Self-Service)** | User Self-Service / Auth Resets / Bot Workflows | `AUTH_PASSWORD_EXPIRED`<br>`ACCOUNT_LOCKED`<br>`CAPTCHA_REQUIRED` | Automated chatbot macro / self-serve password or unlock portal link. |
| **Level 1 (L1 / Service Desk)** | Client-Side Validation & Standard User Errors | `400 Bad Request`<br>`401 Unauthorized`<br>`INVALID_FORM_INPUT`<br>`FILE_MAX_SIZE_EXCEEDED` | Standard SOP script, user guidance, password/MFA reset, browser cache refresh. |
| **Level 2 (L2 / Technical App Support)** | Data Integrity, Permission Sync, API Timeouts, DB Locks | `403 Forbidden`<br>`404 Not Found`<br>`409 Conflict`<br>`DB_DEADLOCK_DETECTED`<br>`TOKEN_EXPIRED` | Log analysis, read-only SQL queries, cache clearing, connection pool bounce. |
| **Level 3 (L3 / Engineering)** | Unhandled Exceptions & Source Code Bugs | `500 Internal Server Error`<br>`NullPointerException`<br>`SEGFAULT`<br>`OUT_OF_MEMORY`<br>`DATA_CORRUPTION_ERR` | Source code fix, emergency hotfix patch deployment, RCA logging. |
| **Level 4 (L4 / External Vendor / OEM)** | Cloud Infrastructure / Hardware / Third-Party SDK | `502 Bad Gateway`<br>`503 Service Unavailable`<br>`AWS_S3_ACCESS_DENIED`<br>`VENDOR_SDK_PANIC` | Underpinning Contract (UC) ticket opening with cloud/vendor provider. |

---

## 3. Escalation Decision Tree for Code Scanning

```
                       [Discovered Error/Exception]
                                     |
           +-------------------------+-------------------------+
           |                                                   |
   (Is Client/User Error?)                           (Is System/Server Error?)
           |                                                   |
     +-----+-----+                                       +-----+-----+
     |           |                                       |           |
  (Self-Svc)  (Validation)                             (Internal)   (External/Vendor)
     |           |                                       |           |
     v           v                                       v           v
    L0          L1                                  +----+----+     L4
                                                    |         |
                                               (Data/DB)  (Code Defect)
                                                    |         |
                                                    v         v
                                                   L2        L3
```
