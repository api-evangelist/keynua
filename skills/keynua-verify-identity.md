---
name: Run a Keynua identity verification
description: Create a standalone biometric identity verification and read its result.
api: https://keynua.github.io/slate
base_url: https://api.keynua.com
operations:
  - POST /identity-verification/v1
  - GET /identity-verification/v1
  - GET /identity-verification/v1/{verificationId}
---

# Run a Keynua identity verification

Use this to verify a person's identity (ID document + biometrics) without a full signing contract.

## Auth
Send `x-api-key` and `authorization` header **Secret** values (see `authentication/keynua-authentication.yml`).

## Steps
1. **Create** — `POST /identity-verification/v1` with the subject and the target document/country (20+ LATAM document types across 20 countries).
2. **Track** — subscribe to webhooks or poll `GET /identity-verification/v1/{verificationId}`.
3. **Read result** — a successful match resolves; failures carry a code such as `NotValidIdCard`, `NotAMatch`, `ScoreTooLow`, or `FacemasksDetected` (see `errors/keynua-error-codes.yml`).
4. **List** — `GET /identity-verification/v1` with `startAt`/`limit` (default 50) to page prior verifications.

## Rules
- Ask subjects for a clear, unmasked, single-face capture in good lighting to avoid `FacemasksDetected` / `ManyFacesDetected` / `ScoreTooLow`.
