---
title: Object Storage
tags: [infrastructure, storage, cloud]
updated: 2026-10-27
---

# Object Storage

Object storage keeps files as whole **objects** in flat **buckets**, each addressed by a key and reached over HTTP. It's how the cloud stores images, backups, videos, logs and static websites cheaply and almost without limit.

## How it works

- A **bucket** is a container with a globally or account-unique name.
- An **object** is a blob of bytes plus metadata, stored under a **key** like `photos/2026/trip.jpg`. The slashes only look like folders; the namespace is flat.
- You read and write objects with HTTP calls (`PUT`, `GET`, `DELETE`). You replace an object rather than editing part of it in place.
- Data is copied across multiple disks or data centers, so losing a server doesn't lose your files.

Amazon S3's API became the de facto standard, so many other services (Cloudflare R2, Backblaze B2, Google Cloud Storage's interoperability mode, MinIO) accept the same API and tools.

## Key terms

- **Storage class**: cheaper tiers for data you rarely read, with slower or costlier retrieval.
- **Egress**: the cost of downloading data out of the provider. Some providers charge for it and some don't, which can dominate the bill.
- **Presigned URL**: a temporary link that lets someone upload or download one object without credentials.
- **Lifecycle rule**: automatically move or delete objects after a set age.
- **Versioning**: keep previous versions when an object is overwritten or deleted.

## In practice

```bash
aws s3 cp backup.tar.gz s3://my-bucket/backups/
aws s3 ls s3://my-bucket/backups/
# The same CLI works with S3-compatible services via --endpoint-url
```

## Common pitfalls

- Accidentally public buckets. Keep buckets private by default and share with presigned URLs.
- Treating it like a disk: there's no cheap rename or append, and listing millions of keys is slow.
- Ignoring egress fees until the bill arrives.

## Related

- [CDNs](cdn.md)
- [Serverless](serverless.md)
