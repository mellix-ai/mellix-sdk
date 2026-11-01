# Changelog

## 0.0.1

First release of the Python (`mellix` on PyPI) and JavaScript/TypeScript (`mellix` on npm) clients for the Mellix Decision API.

- `decide` for `noul`, `choice` and `score` questions, with typed answers and local validation (up to 256 questions a request, 256 options a choice and 4,096 options over a request). Always name the model.
- Images: `assets` on `decide`, the `Image.from_path` / `Image.from_bytes` (Python) and `imageFromPath` / `imageFromBytes` (JavaScript) helpers for PNG and JPEG files up to 16 MiB, and `image_budget` / `imageBudget` (70, 140 or 280).
- The Batch API: `client.batch(lines)` (or `batches.create`), `list`, `retrieve`, `cancel`, `results`, a streaming results reader (`stream_results` / `streamResults`) and `wait_for` / `waitFor`. Results can be downloaded once and are deleted 24 hours after the batch finishes if never downloaded (`BatchResultsDeletedError`).
- `client.admin.keys` manages your organisation's API keys (`list`, `list_all` / `listAll`, `create`, `roll`, `revoke`) with an admin key (`MELLIX_ADMIN_KEY`), sent only to the console.
- `key()`, `models()` and `health()`. Every response's `Mellix-Key-Expires` header is read, with a warning once when 14 days or fewer are left (`on_key_expiry` / `onKeyExpiry` to handle it yourself).
- Retries are never billed twice: every `decide` call and batch submission sends one `Idempotency-Key` on all its attempts. Retries back off on 429, 502, 503 and connection errors, honouring `Retry-After`; 409 `idempotency_in_progress` is waited out.
- Error classes mapped to HTTP status codes, each with the server's error `type`, including `OutOfCreditError`, `PermissionDeniedError`, `NotFoundError`, `ConflictError` and `IdempotencyKeyAlreadyUsedError`.
