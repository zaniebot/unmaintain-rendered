```yaml
number: 7799
title: "Code generator doesn't preserve individual string quote style"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-10-04T04:54:40Z
updated_at: 2025-02-04T13:41:08Z
url: https://github.com/astral-sh/ruff/issues/7799
synced_at: 2026-01-10T11:09:50Z
```

# Code generator doesn't preserve individual string quote style

---

_Issue opened by @dhruvmanila on 2023-10-04 04:54_

This is related to the `Generator` struct.

This is especially true for triple-quoted strings. The `Generator` will normalize them to single quotes and perform some transformations which may or may not be correct:
* Escape the inner quotes if they match the outer ones
* Preserve the whitespaces (for example, using `\n`)

Related issues:
* https://github.com/astral-sh/ruff/issues/6988
* https://github.com/astral-sh/ruff/issues/7736
* https://github.com/astral-sh/ruff/issues/7720

---

_Label `bug` added by @dhruvmanila on 2023-10-04 04:54_

---

_Label `autofix` added by @dhruvmanila on 2023-10-04 04:54_

---

_Comment by @sanxiyn on 2024-02-15 13:37_

I am interested in working on this.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-05-06 12:16_

---

_Assigned to @ntBre by @ntBre on 2025-01-24 15:51_

---

_Unassigned @AlexWaygood by @ntBre on 2025-01-24 15:51_

---

_Closed by @ntBre on 2025-02-04 13:41_

---
