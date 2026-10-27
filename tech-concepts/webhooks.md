---
title: Webhooks
tags: [web, apis, automation]
updated: 2026-10-27
---

# Webhooks

A webhook is an HTTP request one service sends to another when something happens. Instead of you asking "anything new?" every few minutes (**polling**), the service calls you the moment an event occurs.

## How it works

1. You give the provider a URL you control and choose which events you care about (a payment succeeded, a pull request opened).
2. When the event happens, the provider sends a `POST` request to that URL with a JSON description of it.
3. Your endpoint checks the request is genuine, does its work (or queues it), and replies with a `2xx` status quickly.
4. If your endpoint fails or times out, most providers **retry** with increasing delays.

## Key terms

- **Payload**: the JSON body describing the event.
- **Signature**: a hash of the payload made with a shared secret (usually HMAC-SHA256), sent in a header so you can prove the request came from the provider.
- **Idempotency / event ID**: retries mean the same event can arrive more than once; store IDs you've handled and skip repeats.
- **Polling vs. push**: webhooks are push; they save requests and arrive faster.

## In practice

Verifying a signature in Python:

```python
import hmac, hashlib

def is_valid(body: bytes, header_sig: str, secret: str) -> bool:
    expected = hmac.new(secret.encode(), body, hashlib.sha256).hexdigest()
    return hmac.compare_digest(expected, header_sig)
```

Check the provider's docs for the exact header name and format (some prefix it, like `sha256=…`). For local testing, tools such as `ngrok` or `cloudflared` expose a local server at a public URL.

## Common pitfalls

- Skipping signature checks, so anyone who finds the URL can trigger your code.
- Doing slow work before responding, causing timeouts and duplicate retries. Respond first, process in the background.
- Assuming events arrive in order.

## Related

- [OAuth 2.0](oauth.md)
- [Serverless](serverless.md)
- [CI/CD](ci-cd.md)
