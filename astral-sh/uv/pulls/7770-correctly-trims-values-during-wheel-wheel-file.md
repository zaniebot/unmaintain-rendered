```yaml
number: 7770
title: Correctly trims values during wheel WHEEL file parsing
type: pull_request
state: merged
author: Coruscant11
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/wheel-file-trimming
created_at: 2024-09-28T23:55:10Z
updated_at: 2024-09-29T00:08:25Z
url: https://github.com/astral-sh/uv/pull/7770
synced_at: 2026-01-10T12:53:55Z
```

# Correctly trims values during wheel WHEEL file parsing

---

_Pull request opened by @Coruscant11 on 2024-09-28 23:55_



## Summary

My last changes (#6616) used by mistake == instead of !=. :disappointed_relieved: Making values currently never trimmed despite what we wanted.
Values should now be trimmed if needed.

Also removes the trim of the header name, because if a header contains spaces, the header will be skipped by the mailparse crate in the first place.

## Test Plan
- A unit test has been added to validate that we correctly trim values.
- A unit test has been added to validate the header names containing spaces are skipped.


---

_Renamed from "Trims correctly values during wheel WHEEL file parsing" to "Correctly trims values during wheel WHEEL file parsing" by @Coruscant11 on 2024-09-28 23:56_

---

_@charliermarsh approved on 2024-09-29 00:08_

Oops, my bad too! Thanks for following up.

---

_Merged by @charliermarsh on 2024-09-29 00:08_

---

_Closed by @charliermarsh on 2024-09-29 00:08_

---

_Label `bug` added by @charliermarsh on 2024-09-29 00:08_

---
