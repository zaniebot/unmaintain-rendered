```yaml
number: 13478
title: Apply first set of Rustfmt edition 2024 changes
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cargo-fmt-2024-1
created_at: 2025-05-15T19:33:12Z
updated_at: 2025-05-17T00:19:04Z
url: https://github.com/astral-sh/uv/pull/13478
synced_at: 2026-01-12T16:10:42Z
```

# Apply first set of Rustfmt edition 2024 changes

---

_@konstin_

Rustfmt introduces a lot of formatting changes in the 2024 edition. To not break everything all at once, we split out the set of formatting changes compatible with both the 2021 and 2024 edition by first formatting with the 2024 style, and then again with the currently used 2021 style.

Notable changes are the formatting of derive macro attributes and lines with overly long strings and adding trailing semicolons after statements consistently.


---

_Label `internal` added by @konstin on 2025-05-15 19:33_

---

_@charliermarsh approved on 2025-05-17 00:16_

---

_Merged by @charliermarsh on 2025-05-17 00:19_

---

_Closed by @charliermarsh on 2025-05-17 00:19_

---

_Branch deleted on 2025-05-17 00:19_

---
