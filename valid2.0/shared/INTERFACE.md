# Final-graph scientific relation audit interface 2.0.0

Current dataset release: `valid2.0`. It restores all 250 articles in the frozen 1.0.0
order and B01–B05 assignment. Only `final-graph` is in scope. All eligible
`exhibits`, `is_variant_of`, and `explained_by` records are retained without edge
subsampling; `reports` and the actual repository predicate `uses_mechanism` are both
excluded. The user-written `use_machanism` was resolved to `uses_mechanism` before
release freeze. See `release-migration.json`.

Read `dataset.json` first, then `sample-manifest.json`. Paths below are relative to `valid/`. This release reads legacy entity/edge JSON; it does not implement or change current CCS contracts.

## Files and ownership

- `shared/dataset.json`: release binding, file SHA-256 inventory, input/code provenance. `dataset_hash` is SHA-256 of this descriptor without its own hash field, serialized as sorted-key UTF-8 JSON with compact separators and no ASCII escaping. File hashes are over exact bytes.
- `shared/sample-manifest.json`: the only authorized article order, year allocation and B01–B05 assignment. `sample_manifest_hash` is the file-byte SHA-256.
- `shared/articles.jsonl`: original WoS title/abstract and source candidates; `source.excel_row` is a one-based spreadsheet row including header. Extracted title/abstract are retained separately. Any fallback is explicitly marked `text_authority=extraction_fallback_unverified`.
- `final-graph/data/articles.jsonl`: 250 articles in frozen order, including zero-relation articles. Article-level `scientific_record_count` is the definite audit denominator.
- `final-graph/data/records.jsonl`: final-graph audit records for the three included predicates. Records require exact source ID attribution.
- `{stage}/data/source-pending.jsonl`: graph-connected records with missing/unknown provenance; excluded from the definite denominator. Known edges attributed exclusively to other articles are excluded entirely.
- `{stage}/data/nodes.jsonl`: complete raw endpoint payloads, no truncation or property filtering. Use `subject_refs` / `object_refs` to join by `node_ref`. Arrays preserve duplicate node IDs; empty arrays represent missing endpoints.
- There is no active extraction data or paired-candidate product in valid2.0. Those assets remain recoverable under `archive/dataset-valid1.0/` and are not report inputs.

This conversation owns shared, tools, tests, report and both stage `data/` directories. Other conversations own their stage page, `results/`, and stage statistics. Source data is immutable once this release is published. A change requires a new version and an explicit old-to-new result migration review; never resample independently or silently bind old decisions to new data.

## Record identity and provenance

`stage` is `final-graph`. `record_id` is the stage plus SHA-256 of canonical `[stage, article_id, locator]`. The locator contains a worktree-relative path and zero-based JSON Pointer. Edge array positions distinguish duplicate edges; multi-article graph edges produce one record per selected article. Identity is scoped to the bound dataset, not a cross-version semantic identity.

`raw_edge` preserves every original field. `source_article_ids` splits the observed legacy comma-separated source representation (JSON arrays also accepted; semicolons inside complete identifiers are preserved); matching uses entire IDs, preserves case, and never uses substrings. `reports` paths only support context/flags. `endpoint_without_reports_path` does not remove an explicitly source-attributed edge.

Endpoint properties can include evidence from multiple articles after merging. Display the complete payload and distinguish its `source_article` values; never treat a shared node's entire payload as supported by the currently selected abstract. `wos_extraction_text_difference` is a text difference flag, not a scientific error classification.

## Portable review result protocol

Use one JSON envelope per reviewer export. Copy the four binding fields from `dataset.json`: `interface_version`, `dataset_version`, `dataset_hash`, `sample_manifest_hash`. Include `stage`, `reviews` and `omissions`. The Python validator is authoritative. An empty review list means no reviewed records. Missing records and `null` human reviews are unreviewed, never correct by default.

Each review binds `record_id`, `article_id`, `batch_id`, and one `human_review` object. An object has:

| Field | Meaning |
| --- | --- |
| status | `draft` or `reviewed`; a draft does not count as completed |
| decision | `supported`, `partially_supported`, `unsupported`; null only for draft |
| evidence | Literal source passage(s); empty string permitted if unavailable; explain absence in reason |
| reason | Support, direction, semantics, attributes/conditions and stage-change assessment; empty string permitted in public redacted results |
| suggestion | Suggested correction or follow-up; empty string permitted, including in public redacted results |
| reviewer | Explicit human reviewer identity; required when reviewed |
| reviewed_at | ISO-8601 timestamp with timezone when retained; empty string permitted in public redacted results |
| error_types | Array of reviewer-supplied error labels; empty array permitted |

Human fields start null for new records. valid2.0 has no Agent pre-review track. A relation that lacks sufficient source evidence is recorded as `unsupported`; the public redacted result may retain its classification only in `error_types` while leaving `reason`, `suggestion`, and `reviewed_at` empty. This remains distinct from an unreviewed record. A review of a source-pending record remains outside the definite denominator unless a new dataset explicitly resolves provenance.

Omissions use `omission:<UUID>` IDs, an existing `article_id`, free-text `proposed_relation`, and one `human_review` object. They remain outside the existing-edge denominator. Human reference completeness is not established merely by collecting omissions, so strict recall cannot be claimed.

Save files under `{stage}/results/{stage}__{reviewer-slug}__{YYYYMMDDTHHMMSSZ}.json`. A file is a partial snapshot; retain explicit user confirmation when replacing loaded progress with a different snapshot. Across files, never silently choose the latest human decision when two reviewers disagree. For report aggregation, select one canonical snapshot explicitly per stage.

Validate a saved export from the worktree root:

```bash
python valid/tools/review_results.py valid/final-graph/results/<filename>.json
```

The validator checks all version/hash bindings, known stage/article/batch/record IDs, duplicate IDs, field types, decision vocabulary, timezone and reviewed-field completeness. It reads files only. JSON Schema is a structural companion; cross-file binding checks require the Python validator or equivalent page logic.

## Page requirements and handoff completion

The final-graph page must navigate all articles and batches in frozen order, display original title and full abstract plus every relation and endpoint property, keep source-pending records visibly separate, and show missing endpoints/zero-relation articles. It must provide editable review fields and explicit saved/unsaved state. Export a portable JSON file, validate and reload it, and demonstrate restored progress after reopening. Browser storage can assist recovery but is not the sole store. Static HTML cannot automatically write arbitrary filesystem paths without user-granted browser capabilities.

Each stage returns its page path, validated canonical result path, reviewed/unreviewed counts, omissions and persistence test evidence. This conversation then updates `report/validation-report.md`; it does not overwrite stage results.
