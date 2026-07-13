# cmdb-universal-data

SwipeLeft AI's universal CMDB knowledge — the content that is true for
every customer and specific to none.

**This repo is public by design.** Every ServiceMatch deployment runs
inside the customer's own tenant and fetches this content anonymously
(no GitHub credentials ever live in a customer tenant). Backends
degrade gracefully: live fetch → disk cache → bundled fallback.

## Contents

- `discovery_sources.json` — the trusted discovery-source catalog
  (canonical tokens + aliases + intent). Consumer:
  `back_end/cmdb/discovery_sources_catalog.py`.
- `corpus/` — the universal rule corpus (manufacturer aliases, model
  patterns, OS normalization). Consumer:
  `mcp_config_server/tools/download_universal_rule_corpus.py`
  (fetches by release tag; `latest` resolves via GitHub releases).
- `manifest.json` — corpus section index + hashes.

## Hard rules

1. **Universal content only.** Nothing customer-specific, ever — no
   customer names, hostnames, serials, or data derived from a
   customer estate. Customer-tier knowledge lives in the Expertise
   Rule Store, never here.
2. **No secrets.** Public repo; treat every commit as world-readable
   forever.
3. `ad_computers` is deliberately absent from the catalog — Active
   Directory is merge-only corroboration and may never originate CIs.

History: recreated 2026-07-13 after the original was lost in a repo
cleanup (fleet ran silently on the bundled fallback; corpus recovered
from the archived `servicematch-universal-rules`).
