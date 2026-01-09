---
number: 14070
title: "Exit with error code if `uv sync --dry-run` would change anything"
type: issue
state: closed
author: sewi-cpan
labels:
  - enhancement
assignees: []
created_at: 2025-06-16T08:45:21Z
updated_at: 2025-06-16T08:49:32Z
url: https://github.com/astral-sh/uv/issues/14070
synced_at: 2026-01-07T13:12:18-06:00
---

# Exit with error code if `uv sync --dry-run` would change anything

---

_Issue opened by @sewi-cpan on 2025-06-16 08:45_

### Summary

`uv sync --dry-run` should set an exit code if changes would be applied.

This change would help during CI testing because `uv sync --dry-run | grep 'Would make no changes'` is hacky and error-prone.

### Example

_No response_

---

_Label `enhancement` added by @sewi-cpan on 2025-06-16 08:45_

---

_Comment by @konstin on 2025-06-16 08:46_

Do you need this for sync or are you looking for `uv lock --locked`?

---

_Closed by @sewi-cpan on 2025-06-16 08:49_

---

_Comment by @sewi-cpan on 2025-06-16 08:49_

I didn't see `--locked`. Thank you for the hint!

---
