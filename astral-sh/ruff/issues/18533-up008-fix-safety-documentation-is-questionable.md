```yaml
number: 18533
title: UP008 fix safety documentation is questionable
type: issue
state: closed
author: dscorbett
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-06-07T13:19:28Z
updated_at: 2025-06-30T13:42:06Z
url: https://github.com/astral-sh/ruff/issues/18533
synced_at: 2026-01-12T15:54:56Z
```

# UP008 fix safety documentation is questionable

---

_@dscorbett_

### Summary

The documentation of [`super-call-with-parameters` (UP008)](https://docs.astral.sh/ruff/rules/super-call-with-parameters/#fix-safety) says:
> This rule's fix is marked as unsafe because removing the arguments from a call may delete comments that are attached to the arguments.

If that is the only reason, then the fix should be marked safe when there are no comments to delete. Otherwise, the other reasons should be documented.

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `documentation` added by @ntBre on 2025-06-07 15:03_

---

_Label `help wanted` added by @ntBre on 2025-06-10 16:57_

---

_Closed by @ntBre on 2025-06-30 13:42_

---
