---
title: HTTPS and TLS
tags: [security, web, networking]
updated: 2026-10-27
---

# HTTPS and TLS

HTTPS is ordinary HTTP sent inside a **TLS** connection. TLS gives three guarantees: the traffic is **encrypted** (others can't read it), it's **intact** (others can't change it unnoticed), and the server is **authenticated** (you're talking to the real owner of the domain).

## How it works

1. **Handshake.** The client says which TLS versions and ciphers it supports. The server replies with its choice and its **certificate**.
2. **Certificate check.** The certificate binds the domain name to a public key and is signed by a **certificate authority (CA)**. The client checks the signature chain up to a CA it already trusts, the expiry date, and that the name matches the site.
3. **Key exchange.** Both sides agree on a shared secret (in TLS 1.3, using ephemeral Diffie-Hellman), so even someone who later steals the server's private key can't decrypt past sessions (**forward secrecy**).
4. **Encrypted traffic.** Everything after that, including the HTTP request path and headers, is encrypted with fast symmetric keys derived from the shared secret.

TLS 1.3 needs one round trip to set up, versus two for TLS 1.2.

## Key terms

- **Certificate / CA**: the signed ID card, and the issuer that signs it.
- **Chain of trust**: server certificate → intermediate CA → root CA stored in your OS or browser.
- **Let's Encrypt / ACME**: a free CA and the protocol for getting and renewing certificates automatically.
- **SNI**: the hostname sent at the start of the handshake so one IP can serve certificates for many sites.
- **HSTS**: a header telling browsers to only ever use HTTPS for this site.

## In practice

```bash
curl -vI https://example.com                     # see the handshake and certificate
openssl s_client -connect example.com:443 -servername example.com
```

Hosts like Vercel, Netlify and Cloudflare issue and renew certificates automatically once DNS points at them.

## Common pitfalls

- Expired certificates when renewal isn't automated.
- **Mixed content**: an HTTPS page loading scripts or images over plain HTTP.
- Assuming HTTPS hides everything: the domain you visit is still visible via DNS and SNI unless those are encrypted too.

## Related

- [DNS](dns.md)
- [CDNs](cdn.md)
- [OAuth 2.0](oauth.md)
