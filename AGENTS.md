# Civitai Browser Neo Extension Notes

## Recent API Constraints (August 2026)

Recently, the Civitai API has implemented much stricter rate limits and request checks behind Cloudflare. This causes multiple endpoints, such as `api/v1/models` and `api/v1/model-versions`, to return a `503 Service Unavailable` with the JSON payload:
```json
{"error":"Model search is temporarily overloaded — please retry."}
```
Cloudflare also serves a `Retry-After` header when this occurs.

## Fixes Implemented

1. **Exponential Backoff and Retry Wrapper**:
   All direct `requests.get` calls to the Civitai API have been wrapped in `requests_get_with_retry` (defined in `scripts/civitai_api.py`). This wrapper listens for `429`, `502`, `503`, and `504` status codes. Upon hitting these, it parses the `Retry-After` header. If present, it sleeps for that duration; if not, it uses an exponential backoff formula (`base_backoff * 2^(attempt-1)`). The wrapper ensures transient server overloads don't result in immediate hard failures for the end-user.

2. **Batch Size Reduction**:
   The `limit` query parameters were commonly set to `100` for batch-fetching. To reduce load and the likelihood of triggering rate limits, all `limit=100` instances in the background model-refreshing routines (`civitai_file_manage.py`) have been reduced to `limit=50`. In addition, `civitai_api.py:create_api_url()` caps user-supplied `tile_count` at 50 max before fetching from the API.

These changes collectively improve reliability and align the extension's network behavior with standard REST API developer guidelines.
