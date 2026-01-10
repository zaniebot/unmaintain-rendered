```yaml
number: 5063
title: "Add `OptionsMetadata` macro to uv"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: charlie/docs-metadata
created_at: 2024-07-15T01:56:15Z
updated_at: 2024-07-15T19:24:09Z
url: https://github.com/astral-sh/uv/pull/5063
synced_at: 2026-01-10T13:42:52Z
```

# Add `OptionsMetadata` macro to uv

---

_Pull request opened by @charliermarsh on 2024-07-15 01:56_

## Summary

The bulk of the change is copied directly from Ruff:

- https://github.com/astral-sh/ruff/blob/dc8db1afb08704ad6a788c497068b01edf8b460d/crates/ruff_workspace/src/options_base.rs
- https://github.com/astral-sh/ruff/blob/dc8db1afb08704ad6a788c497068b01edf8b460d/crates/ruff_macros/src/config.rs


---

_Review requested from @zanieb by @charliermarsh on 2024-07-15 02:00_

---

_Marked ready for review by @charliermarsh on 2024-07-15 02:00_

---

_Label `documentation` added by @charliermarsh on 2024-07-15 02:00_

---

_Label `internal` added by @charliermarsh on 2024-07-15 02:00_

---

_Comment by @charliermarsh on 2024-07-15 02:11_

Outstanding items:

1. Writing out the actual documentation for each option.
2. Generating the API reference from the macro-extracted metadata (both for options and for things like `tool.uv.workspace`).
3. Publishing the Ruff and uv documentation as one unified site (likely via a separate repo that pulls in both).

---

_Merged by @charliermarsh on 2024-07-15 19:24_

---

_Closed by @charliermarsh on 2024-07-15 19:24_

---

_Branch deleted on 2024-07-15 19:24_

---
