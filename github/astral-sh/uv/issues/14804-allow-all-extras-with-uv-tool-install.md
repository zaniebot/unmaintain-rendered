---
number: 14804
title: "allow `--all-extras` with `uv tool install`"
type: issue
state: open
author: sslivkoff
labels:
  - enhancement
assignees: []
created_at: 2025-07-22T05:42:25Z
updated_at: 2025-07-22T12:24:12Z
url: https://github.com/astral-sh/uv/issues/14804
synced_at: 2026-01-07T13:12:19-06:00
---

# allow `--all-extras` with `uv tool install`

---

_Issue opened by @sslivkoff on 2025-07-22 05:42_

### Summary

`uv tool install --all-extras ...` would be very nice syntax for packages that have lots of extras and you just want them all

these commands already work:
- `uv install --all-extras ...`
- `uv tool install package[extra1,extra2,extra3,extra4,extra5]`
- `uv tool install --editable .[extra1,extra2,extra3,extra4,extra5]`

so it would be very nice syntactic sugar to be able to just do `uv tool install package --all-extras` so that you don't have to remember the names of all the extras and don't have to type them out

### Example

_No response_

---

_Label `enhancement` added by @sslivkoff on 2025-07-22 05:42_

---

_Comment by @zanieb on 2025-07-22 11:21_

This seems reasonable to me.

---

_Label `good first issue` added by @zanieb on 2025-07-22 11:21_

---

_Comment by @charliermarsh on 2025-07-22 12:15_

Probably quite a bit harder than `--all-extras` in other contexts since we can't know the extras until we've resolved the tool metadata, so this would somehow need to be wired up to the resolver. (Not impossible but unsure if it's a good first issue.)

---

_Comment by @zanieb on 2025-07-22 12:24_

Ah, that makes sense.

---

_Label `good first issue` removed by @zanieb on 2025-07-22 12:24_

---
