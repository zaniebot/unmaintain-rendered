```yaml
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
synced_at: 2026-01-10T04:36:20Z
```

# Support resolution strategy for python version 

---

_Issue opened by @T-256 on 2024-07-30 16:38_

Here is what I can suggest:

`--python-resolution` which applies to provided constraints (e.g. ">=3.8"):
- `default`: current behavior - uses already installed python which satisfies OR download highest available.
- `highest`: force use highest and try download if not installed. fallback to `default` if maximum number not explicitly set in version-specifier.
- `lowest`: force use lowest and try download if not installed. fallback to `default` if minimum number not explicitly set in version-specifier. always chooses highest resolved version patch numbers.


---

_Label `needs-decision` added by @zanieb on 2024-07-30 16:40_

---

_Label `cli` added by @zanieb on 2024-07-30 16:40_

---

_Comment by @zanieb on 2024-11-08 17:03_

See also:

- https://github.com/astral-sh/uv/issues/8247
- https://github.com/astral-sh/uv/issues/7562
- https://github.com/astral-sh/uv/issues/7779


---
