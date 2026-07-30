---
name: Send and verify a Keynua OTP
description: Send a one-time password to a user and verify their response.
api: https://keynua.github.io/slate
base_url: https://api.keynua.com
operations:
  - POST /otp/v1/send
  - POST /otp/v1/verify
  - GET /otp/v1/info
---

# Send and verify a Keynua OTP

Use this to add a one-time-password challenge (delivered over WhatsApp/SMS/email).

## Auth
Send `x-api-key` and `authorization` header **Secret** values (see `authentication/keynua-authentication.yml`).

## Steps
1. **Send** — `POST /otp/v1/send` with the recipient and delivery channel.
2. **Inspect** (optional) — `GET /otp/v1/info` to check OTP state.
3. **Verify** — `POST /otp/v1/verify` with the code the user entered.

## Rules
- Treat the code as short-lived; on a failed verify, resend rather than reusing the prior challenge.
