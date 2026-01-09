---
number: 22395
title: no-explicit-stacklevel (B028) should allow no stacklevel when skip_file_prefixes is provided
type: issue
state: open
author: abrahammurciano
labels: []
assignees: []
created_at: 2026-01-05T09:17:16Z
updated_at: 2026-01-05T09:18:06Z
url: https://github.com/astral-sh/ruff/issues/22395
synced_at: 2026-01-07T13:12:16-06:00
---

# no-explicit-stacklevel (B028) should allow no stacklevel when skip_file_prefixes is provided

---

_Issue opened by @abrahammurciano on 2026-01-05 09:17_

### Summary

According to the [python documentation](https://docs.python.org/3/library/warnings.html#warnings.warn)

> The `skip_file_prefixes` keyword argument can be used to indicate which stack frames are ignored when counting stack levels. [...] When prefixes are supplied, `stacklevel` is implicitly overridden to be `max(2, stacklevel)`.
> [...]
> Changed in version 3.12: Added skip_file_prefixes.

If `skip_file_prefixes` is provided, then the stacklevel is already set to at least 2, and the user has already explicitly skipped certain stack frames by prefix. The resulting warning will already give the caller more context about the warning since it will output a stack frame higher up than the one the warning was issued from, most likely outside of the issuing package (i.e. the caller's actual call).

Therefore I believe [the B028 rule](https://docs.astral.sh/ruff/rules/no-explicit-stacklevel/) should be updated to allow either of the two parameters instead of only `stacklevel`.

---
