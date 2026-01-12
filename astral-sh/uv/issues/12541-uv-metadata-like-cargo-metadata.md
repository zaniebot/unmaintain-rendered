```yaml
number: 12541
title: uv metadata (like cargo metadata)
type: issue
state: open
author: heiner
labels:
  - enhancement
assignees: []
created_at: 2025-03-29T00:31:14Z
updated_at: 2025-03-29T00:31:14Z
url: https://github.com/astral-sh/uv/issues/12541
synced_at: 2026-01-12T16:01:05Z
```

# uv metadata (like cargo metadata)

---

_@heiner_

### Summary

For some use cases, it's useful to have Python package dependencies in a machine-readable form. This could work like `uv tree`, but output a format like json. Cargo has this as `cargo metadata`; it could also be another type of `uv export` format.

We would use this, for instance, to know which parts of a larger monorepo need to be made available for building in an external service.

When developing on Darwin but running in prod on Linux, this would ideally include a way to resolve "as if on Linux".

### Example

_No response_

---

_Label `enhancement` added by @heiner on 2025-03-29 00:31_

---
