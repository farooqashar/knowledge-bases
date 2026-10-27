---
title: OAuth 2.0
tags: [security, apis, web]
updated: 2026-10-27
---

# OAuth 2.0

OAuth 2.0 lets you give an app limited access to your account on another service without handing over your password. "Allow this app to read your Google Calendar?" is OAuth.

## How it works

The common **authorization code flow**:

1. The app sends you to the service's sign-in page with what it wants (**scopes**, such as `calendar.readonly`) and where to send you back (**redirect URI**).
2. You sign in on the service's own page and approve.
3. The service redirects you back to the app with a short-lived **authorization code**.
4. The app's server exchanges that code (plus its client secret, or a PKCE verifier) for an **access token**.
5. The app calls the API with `Authorization: Bearer <access token>`. When the token expires, it uses a **refresh token** to get a new one without asking you again.

The app never sees your password, and you can revoke its access at any time from the service's settings.

## Key terms

- **Scope**: what the token is allowed to do. Ask for the least you need.
- **Access token / refresh token**: short-lived key for API calls / long-lived key for getting new access tokens.
- **Client ID / client secret**: the app's identity with the service.
- **PKCE**: an extra check that protects the flow for apps that can't keep a secret (mobile apps, single-page apps).
- **OpenID Connect (OIDC)**: a layer on top of OAuth that also tells the app *who* you are, which is what "Sign in with Google" uses.

## Common pitfalls

- Treating OAuth as login. OAuth alone is about access; use OIDC for identity.
- Requesting broad scopes "just in case," which scares users and widens damage if a token leaks.
- Storing tokens in places scripts can read, such as `localStorage` in a browser.

## Related

- [HTTPS and TLS](https-and-tls.md)
- [Webhooks](webhooks.md)
- [Model Context Protocol](mcp.md)
