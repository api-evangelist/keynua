---
name: Create and sign a Keynua contract
description: Create a contract, deliver it to signers over WhatsApp/SMS/email, collect signing media, and confirm completion.
api: https://keynua.github.io/slate
base_url: https://api.keynua.com
operations:
  - PUT /contracts/v1
  - POST /contracts/v1/sign
  - POST /contracts/v1/sign/upload
  - PUT /contracts/v2/sign/batch
  - GET /contracts/v1/{contractId}
---

# Create and sign a Keynua contract

Use this to run an end-to-end electronic-signature flow with Keynua.

## Auth
Send both headers on every request (see `authentication/keynua-authentication.yml`):
- `x-api-key`: your API Key **Secret** value
- `authorization`: your API Token **Secret** value

Use `https://api.stg.keynua.com` while testing, `https://api.keynua.com` in production.

## Steps
1. **Create the contract** — `PUT /contracts/v1` with the documents (max 5, ≤1MB each) and signers (max 4). Choose the delivery channel (WhatsApp, SMS, or email). Response returns the `contractId`.
2. **Track state** — poll `GET /contracts/v1/{contractId}` or (preferred) subscribe to webhooks (`ItemWorking`, `ItemSuccess`, `ItemError`, `ContractFinished`). Verify `Keynua-Signature` / `Keynua-Signature-V2` on every webhook.
3. **Signer collects pending items** — `POST /contracts/v1/sign` returns the pending items for the signer.
4. **Upload signing media** — for each media item, `POST /contracts/v1/sign/upload` to get signed upload credentials, then `PUT` the file to the returned URL (videos ≤15s).
5. **Submit the batch** — `PUT /contracts/v2/sign/batch` with the signing information.
6. **Confirm** — wait for the `ContractFinished` webhook (or `GET /contracts/v1/{contractId}` state `done`). Signed PDF URLs are signed and expire after 12 hours.

## Rules
- Not idempotent: retrying `PUT /contracts/v1` creates a new contract. Re-submitting an item increments its version.
- On an `ItemError` webhook, read the error code (see `errors/keynua-error-codes.yml`) and ask the signer to recapture.
