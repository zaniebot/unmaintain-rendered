```yaml
number: 13669
title: "Reduce number of reference-checks for `uv cache clean`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/find
created_at: 2025-05-27T01:31:03Z
updated_at: 2025-05-27T01:43:26Z
url: https://github.com/astral-sh/uv/pull/13669
synced_at: 2026-01-10T11:10:42Z
```

# Reduce number of reference-checks for `uv cache clean`

---

_Pull request opened by @charliermarsh on 2025-05-27 01:31_

## Summary

This should reduce the number of filesystem operations fairly dramatically:

- Only query actual symlinks.
- Don't recurse into package bodies (huge).
- Only traverse once (rather than twice).

Closes https://github.com/astral-sh/uv/issues/13667.


---

_Renamed from "Reduce number of reference-checks for uv cache clean" to "Reduce number of reference-checks for `uv cache clean`" by @charliermarsh on 2025-05-27 01:31_

---

_Label `performance` added by @charliermarsh on 2025-05-27 01:31_

---

_Merged by @charliermarsh on 2025-05-27 01:43_

---

_Closed by @charliermarsh on 2025-05-27 01:43_

---

_Branch deleted on 2025-05-27 01:43_

---
