---
number: 5609
title: Support resolution strategy for python version 
type: issue
state: open
author: T-256
labels:
  - needs-decision
  - cli
assignees: []
created_at: 2024-07-30T16:38:42Z
updated_at: 2024-11-08T17:03:10Z
url: https://github.com/astral-sh/uv/issues/5609
synced_at: 2026-01-07T13:12:17-06:00
---

# Support resolution strategy for python version 

---

_Issue opened by @T-256 on 2024-07-30 16:38_

Here is what I can suggest:

`--python-resolution` which applies to provided constraints (e.g. ">=3.8"):
- `default`: current behavior - uses already installed python which satisfies OR download highest available.
- `highest`: force use highest and try download if not installed. fallback to `default` if maximum number not explicitly set in version-specifier.
- `lowest`: force use lowest and try download if not installed. fallback to `default` if minimum number not explicitly set in version-specifier. always chooses highest resolved version patch numbers.


---

_Referenced in [astral-sh/uv#5592](../../astral-sh/uv/pulls/5592.md) on 2024-07-30 16:39_

---

_Label `needs-decision` added by @zanieb on 2024-07-30 16:40_

---

_Label `cli` added by @zanieb on 2024-07-30 16:40_

---

_Referenced in [astral-sh/uv#8247](../../astral-sh/uv/issues/8247.md) on 2024-11-08 17:00_

---

_Referenced in [astral-sh/uv#7779](../../astral-sh/uv/issues/7779.md) on 2024-11-08 17:02_

---

_Comment by @zanieb on 2024-11-08 17:03_

See also:

- https://github.com/astral-sh/uv/issues/8247
- https://github.com/astral-sh/uv/issues/7562
- https://github.com/astral-sh/uv/issues/7779


---

_Referenced in [astral-sh/uv#7429](../../astral-sh/uv/issues/7429.md) on 2024-12-14 17:49_

---

_Referenced in [astral-sh/uv#12122](../../astral-sh/uv/issues/12122.md) on 2025-03-12 17:53_

---

_Referenced in [astral-sh/setup-uv#557](../../astral-sh/setup-uv/issues/557.md) on 2025-09-08 21:20_

---
