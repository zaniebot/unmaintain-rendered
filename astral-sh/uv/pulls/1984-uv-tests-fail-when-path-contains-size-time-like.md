```yaml
number: 1984
title: Uv tests fail when path contains size/time-like strings
type: pull_request
state: merged
author: sbrugman
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-02-26T13:52:06Z
updated_at: 2024-02-26T14:04:26Z
url: https://github.com/astral-sh/uv/pull/1984
synced_at: 2026-01-12T16:04:49Z
```

# Uv tests fail when path contains size/time-like strings

---

_@sbrugman_

The test suite for `uv` fails when the user path contains substrings that are incorrectly identified as `[TIME]` or `[SIZE]`, e.g.

`/Users/AB10BA/Documents/uv/` => `/Users/AB[SIZE]A/Documents/uv/`

Proposed change expects whitespace in front of the search string.

---

_@konstin approved on 2024-02-26 13:53_

Good catch

---

_Label `internal` added by @MichaReiser on 2024-02-26 13:59_

---

_Merged by @MichaReiser on 2024-02-26 14:02_

---

_Closed by @MichaReiser on 2024-02-26 14:02_

---

_Branch deleted on 2024-02-26 14:04_

---
