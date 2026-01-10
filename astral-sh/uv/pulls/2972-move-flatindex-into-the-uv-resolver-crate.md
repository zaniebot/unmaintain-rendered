```yaml
number: 2972
title: "Move `FlatIndex` into the `uv-resolver` crate"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flat-index
created_at: 2024-04-10T18:22:01Z
updated_at: 2024-04-10T18:38:47Z
url: https://github.com/astral-sh/uv/pull/2972
synced_at: 2026-01-10T14:43:31Z
```

# Move `FlatIndex` into the `uv-resolver` crate

---

_Pull request opened by @charliermarsh on 2024-04-10 18:22_

## Summary

This lets us remove circular dependencies (in the future, e.g., #2945) that arise from `FlatIndex` needing a bunch of resolver-specific abstractions (like incompatibilities, required hashes, etc.) that aren't necessary to _fetch_ the flat index entries.


---

_Label `internal` added by @charliermarsh on 2024-04-10 18:22_

---

_Marked ready for review by @charliermarsh on 2024-04-10 18:22_

---

_Merged by @charliermarsh on 2024-04-10 18:38_

---

_Closed by @charliermarsh on 2024-04-10 18:38_

---

_Branch deleted on 2024-04-10 18:38_

---
