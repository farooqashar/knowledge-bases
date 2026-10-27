---
title: CDNs
tags: [web, networking, performance]
updated: 2026-10-27
---

# CDNs

A Content Delivery Network is a set of servers spread around the world that keep copies of your site's content close to visitors. A request from Tokyo is answered by a server in Tokyo instead of crossing the Pacific to your origin.

## How it works

1. DNS (or anycast routing, where many locations share one IP) sends each visitor to the nearest **edge** location.
2. The edge checks its **cache**. On a **hit**, it answers immediately.
3. On a **miss**, it fetches from your **origin** server, returns the response, and stores a copy for the next visitor.
4. How long it keeps the copy is controlled by HTTP headers, mainly `Cache-Control`.

Modern CDNs also terminate TLS, block attacks, and can run code at the edge (Cloudflare Workers, Vercel Edge Functions).

## Key terms

- **Origin**: your real server or storage bucket.
- **Edge / PoP (point of presence)**: a CDN location.
- **Cache hit ratio**: share of requests answered from cache. Higher means faster and cheaper.
- **Purge / invalidation**: telling the CDN to drop cached copies after you change something.
- **`Cache-Control: max-age=…, s-maxage=…`**: how long browsers and shared caches may reuse a response.

## In practice

```http
Cache-Control: public, max-age=31536000, immutable
```

This header suits files with a content hash in the name (`app.3f9a1c.js`): they never change, so they can be cached for a year. HTML pages usually get a short lifetime or `no-cache` so updates appear quickly.

## Common pitfalls

- Caching personalized pages, so one user's content is served to another. Mark them `private` or don't cache them.
- Deploying new files under old names and wondering why visitors still see the old version.

## Related

- [DNS](dns.md)
- [HTTPS and TLS](https-and-tls.md)
- [Object storage](object-storage.md)
