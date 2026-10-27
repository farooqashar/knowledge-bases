---
title: Serverless
tags: [infrastructure, cloud]
updated: 2026-10-27
---

# Serverless

Serverless means running code without managing servers: you upload a function, the platform runs it when a request or event arrives, scales it automatically, and bills only for the time it actually runs. There are still servers; you just never see them.

## How it works

1. You deploy a function (an HTTP handler, or code triggered by a queue, a schedule, or a file upload).
2. When an event arrives, the platform starts an instance if none is ready, runs your code, and returns the result.
3. Under heavy load it runs many instances in parallel; with no traffic it scales to zero and costs nothing.

Two main styles:

- **Container- or VM-based functions** (AWS Lambda, Google Cloud Run functions, Vercel Functions): full language runtimes, but an idle function may need a **cold start** of tens of milliseconds to seconds.
- **Edge isolates** (Cloudflare Workers, Vercel Edge Functions): lightweight sandboxes that start almost instantly and run close to users, with a more limited runtime.

## Key terms

- **Cold start**: delay when a new instance has to boot.
- **Stateless**: instances don't keep memory between requests; state lives in a database, cache or object storage.
- **Timeout / memory limit**: functions have caps on how long and how big they can run.
- **Trigger**: what invokes the function (HTTP, cron, queue message, file upload).

## In practice

A Cloudflare Worker:

```js
export default {
  async fetch(request) {
    return new Response("Hello from the edge");
  },
};
```

## Common pitfalls

- Long-running or always-on work (websockets, big batch jobs) that fights timeouts; a container or VM fits better.
- Opening a new database connection per request and exhausting the database's connection limit.
- Costs that are tiny at low traffic but surprising at high, steady traffic.

## Related

- [Containers](containers.md)
- [Cron and scheduled jobs](cron.md)
- [Webhooks](webhooks.md)
- [Object storage](object-storage.md)
