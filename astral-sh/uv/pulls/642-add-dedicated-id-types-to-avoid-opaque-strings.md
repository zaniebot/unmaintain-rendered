```yaml
number: 642
title: Add dedicated ID types to avoid opaque strings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/types
created_at: 2023-12-14T00:47:07Z
updated_at: 2023-12-14T00:53:35Z
url: https://github.com/astral-sh/uv/pull/642
synced_at: 2026-01-12T16:04:05Z
```

# Add dedicated ID types to avoid opaque strings

---

_@charliermarsh_

This allows us to enforce type safety within the resolver. For example, in the index, we can remove `String` as a key type and enforce that callers _must_ present us with a `PackageId`. (This actually caught one bug, where we were using the SHA rather than the package ID. That bug shouldn't have had any effect given where it was, since those are 1:1, but it's still problematic.)

---

_Merged by @charliermarsh on 2023-12-14 00:53_

---

_Closed by @charliermarsh on 2023-12-14 00:53_

---

_Branch deleted on 2023-12-14 00:53_

---
