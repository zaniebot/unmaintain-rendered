```yaml
number: 959
title: "consider splitting Rust types for \"Place with metadata from place table\" vs \"Place key from AST node\""
type: issue
state: open
author: carljm
labels:
  - internal
assignees: []
created_at: 2025-08-08T22:05:13Z
updated_at: 2025-08-08T22:06:40Z
url: https://github.com/astral-sh/ty/issues/959
synced_at: 2026-01-10T02:06:24Z
```

# consider splitting Rust types for "Place with metadata from place table" vs "Place key from AST node"

---

_Issue opened by @carljm on 2025-08-08 22:05_

Today in type inference we sometimes construct a Place from an AST node, to use as a key for lookup in the place table. It's confusing and a potential footgun that this uses the same Rust type as a Place queried from the actual Place table, because querying the metadata (flags) on the former will just give you a meaningless/misleading "no" answer. It would be better to use separate Rust types for these, so that if we have a Rust type with place metadata, we know that metadata is actually accurate from the place table, not just misleadingly empty.

---

_Label `internal` added by @carljm on 2025-08-08 22:05_

---
