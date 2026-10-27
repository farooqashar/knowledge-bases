---
title: DNS
tags: [networking, web]
updated: 2026-10-27
---

# DNS

The Domain Name System turns names people can remember (`example.com`) into the IP addresses computers use to connect (`93.184.215.14`). It's a distributed, cached directory: no single server knows every name.

## How it works

When you visit `blog.example.com`:

1. Your device asks a **recursive resolver** (usually run by your ISP, or a public one like `1.1.1.1` or `8.8.8.8`).
2. If the resolver doesn't have the answer cached, it asks a **root server**, which points it to the servers for `.com`.
3. The `.com` **TLD servers** point it to the domain's **authoritative nameservers** (often your DNS provider, such as Cloudflare).
4. The authoritative server returns the record. The resolver caches it for the record's **TTL** and hands it back to your device.

Most lookups never reach step 2, because some cache along the way already has the answer.

## Key terms

- **A / AAAA record**: maps a name to an IPv4 / IPv6 address.
- **CNAME**: an alias from one name to another name (`www` → `example.com`). Can't sit at the root of a domain alongside other records; providers offer "CNAME flattening" or ALIAS records to work around it.
- **MX**: which mail servers accept email for the domain.
- **TXT**: free-form text, used to prove ownership and for email security (SPF, DKIM, DMARC).
- **NS**: which nameservers are authoritative for the domain.
- **TTL**: how many seconds resolvers may cache a record.
- **Registrar vs. DNS host**: the registrar is where you bought the domain; the DNS host serves its records. They can be different companies.

## In practice

```bash
dig example.com A +short        # just the IP
dig example.com MX              # mail servers
dig @1.1.1.1 example.com        # ask a specific resolver
dig +trace example.com          # follow the lookup from the root
```

Pointing a domain at a host like Vercel or Cloudflare Pages usually means adding an A record for the root and a CNAME for subdomains, as the host's dashboard instructs.

## Common pitfalls

- **"It works for me but not for them"**: caches still hold the old record until its TTL expires. Lower the TTL a day before a planned change.
- Editing records at the registrar when the domain's nameservers point somewhere else, so the edits do nothing.
- Forgetting email records (MX, SPF) when moving a domain to a new DNS host.

## Related

- [HTTPS and TLS](https-and-tls.md)
- [CDNs](cdn.md)
