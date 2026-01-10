```yaml
number: 789
title: "Enable LRU for `parsed_module`, `semantic_index` and maybe more queries"
type: issue
state: open
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-07-09T10:39:14Z
updated_at: 2025-12-03T09:05:11Z
url: https://github.com/astral-sh/ty/issues/789
synced_at: 2026-01-10T01:56:40Z
```

# Enable LRU for `parsed_module`, `semantic_index` and maybe more queries

---

_Issue opened by @MichaReiser on 2025-07-09 10:39_

Enable Salsa's LRU eviction for some of the most dominent Salsa queries. This should help reduce memory usage after the initial type checking completed (mainly relevant in the LSP use case)

---

_Label `memory` added by @MichaReiser on 2025-07-09 10:39_

---

_Comment by @MichaReiser on 2025-12-03 09:05_

This requires fixing https://github.com/salsa-rs/salsa/issues/1032 upstream

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-03 09:05_

---
