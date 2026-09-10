# 2DM Knowledge Graph (2022–2026)

[中文说明](README.zh-CN.md)

This repository contains the isolated knowledge graph built from
two-dimensional magnetic-material literature published from 2022 through
2026.

## Dataset

- File: `knowledge_graph.json`
- Nodes: 88,987
- Edges: 235,718
- Size: 109,995,800 bytes
- SHA-256: `46ea8cf5e9c6b2f25168caea4f00723fe8908c4b5da4b39682d6e4ccfd1f707c`

The graph file is stored with Git Large File Storage (Git LFS). Install Git
LFS before cloning or pulling the repository.

## Extraction workflow

1. Read the 2022–2026 Web of Science workbooks and normalize article identifiers,
   including DOI-based duplicate handling.
2. Use each article's title and abstract for topic screening and structured
   extraction of material entities, properties, mechanisms, applications, and
   their relations.
3. Validate the extracted structure, relation endpoints, source attribution,
   and supporting evidence; reject relations that fail these checks.
4. Merge accepted article-level outputs into the isolated graph while retaining
   article provenance, then export `knowledge_graph.json`.

`valid2.0` is a post-extraction human audit of a sample of relations from the
final graph. It is not an extraction stage and does not imply that every graph
relation has been reviewed.

## valid2.0 human-reviewed validation release

The [`valid2.0/`](valid2.0/) directory contains the self-contained validation
release for the final graph: 2,043 relations from 247 source articles, all
human-reviewed by `LYZ`. The decisions are 1,277 supported, 475 partially
supported, and 291 unsupported (85.76% pass rate).

Open `valid2.0/final-graph/index.html` directly in Chromium, Chrome, or Edge
for the offline review page. Verify every release file from the repository root:

```bash
sha256sum --check valid2.0/SHA256SUMS
```
