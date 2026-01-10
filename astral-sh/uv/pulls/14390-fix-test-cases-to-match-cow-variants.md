```yaml
number: 14390
title: Fix test cases to match Cow variants
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/cow-tests
created_at: 2025-07-01T12:41:45Z
updated_at: 2025-07-01T18:39:19Z
url: https://github.com/astral-sh/uv/pull/14390
synced_at: 2026-01-10T06:53:01Z
```

# Fix test cases to match Cow variants

---

_Pull request opened by @jtfmumm on 2025-07-01 12:41_

Updates `without_trailing_slash` and `without_fragment` to separately match values against `Cow` variants.

Closes #14350


---

_Label `internal` added by @jtfmumm on 2025-07-01 12:41_

---

_Comment by @zanieb on 2025-07-01 12:52_

Don't we want to assert we're borrowing here? Why not assert with `matches`?

---

_Renamed from "Remove Cow from test comparisons" to "Fix test cases to match Cow variants" by @zanieb on 2025-07-01 18:39_

---

_Merged by @zanieb on 2025-07-01 18:39_

---

_Closed by @zanieb on 2025-07-01 18:39_

---

_Branch deleted on 2025-07-01 18:39_

---
