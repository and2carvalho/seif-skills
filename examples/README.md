# Examples

## sample-diff.txt

A minimal unified diff to feed to `seif-provenance` for smoke testing.

```bash
curl -X POST https://api.seifprotocol.com/v1/sign \
  -H "Authorization: Bearer $SEIF_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -Rs '{diff: ., model: "test", session_id: "smoke-001"}' < sample-diff.txt)"
```

Expected response (HTTP 201):

```json
{
  "evidence_id": "ev_...",
  "signature": "<base64>",
  "public_key": "<base64>",
  "signed_at": "2026-05-14T20:00:00Z",
  "signed_url": "https://verify.seifprotocol.com/v1/evidence/ev_...",
  "integrity_hash": "<sha256-hex>"
}
```
