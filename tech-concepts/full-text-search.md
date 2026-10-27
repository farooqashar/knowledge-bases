---
title: Full-Text Search
tags: [databases, search]
updated: 2026-10-27
---

# Full-Text Search

Full-text search finds documents that contain words, and ranks them by how well they match, fast enough to search millions of documents as you type. It's what powers search boxes, from a docs site to an inbox.

## How it works

The core data structure is the **inverted index**: instead of storing "document → words," it stores "word → documents that contain it."

```
"cloud"  → doc 2, doc 7, doc 9
"storage"→ doc 2, doc 4
```

1. **Indexing**: each document is **tokenized** into words, normalized (lowercased, accents removed, often **stemmed** so "running" matches "run"), and added to the index.
2. **Querying**: the query goes through the same steps, the engine looks up each term, and combines the document lists.
3. **Ranking**: matches are scored, classically with **BM25**, which rewards terms that appear often in a document but rarely across all documents.

## Key terms

- **Tokenizer / analyzer**: rules for splitting and normalizing text.
- **Typo tolerance**: matching "srvless" to "serverless," usually with edit distance.
- **Facets / filters**: narrowing results by fields like date or category.
- **Semantic (vector) search**: matching by meaning using embeddings instead of exact words. **Hybrid search** combines both.

## In practice

Common engines: **Elasticsearch / OpenSearch** (powerful, heavier to run), **Meilisearch** and **Typesense** (simple, fast, typo-tolerant out of the box), and the built-in full-text features of **SQLite (FTS5)** and **Postgres**, which are often enough for small projects.

```sql
-- SQLite FTS5
CREATE VIRTUAL TABLE notes USING fts5(title, body);
SELECT title FROM notes WHERE notes MATCH 'cloud storage' ORDER BY rank;
```

## Common pitfalls

- Using `LIKE '%word%'` on a large table; it scans every row and can't rank.
- Forgetting to re-index when the source data changes.
- Reaching for a separate search server when your database's built-in search would do.

## Related

- [Object storage](object-storage.md)
