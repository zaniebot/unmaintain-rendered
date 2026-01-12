```yaml
number: 5456
title: Add more deprecated numpy functions to NPY
type: issue
state: closed
author: hoxbro
labels:
  - rule
assignees: []
created_at: 2023-07-01T12:49:42Z
updated_at: 2023-07-03T00:50:16Z
url: https://github.com/astral-sh/ruff/issues/5456
synced_at: 2026-01-12T15:54:45Z
```

# Add more deprecated numpy functions to NPY

---

_@hoxbro_

With the release of Numpy 1.25, more functions have been deprecated; see [here](https://numpy.org/doc/stable/release/1.25.0-notes.html#deprecations).  The following deprecations have a direct replacement so it could be nice to add them to NPY.

`np.round_` --> `np.round`
`np.product` --> `np.prod`
`np.cumproduct` --> `np.cumprod`
`np.sometrue` --> `np.any`
`np.alltrue` --> `np.all`





---

_Label `rule` added by @charliermarsh on 2023-07-03 00:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-03 00:18_

---

_Closed by @charliermarsh on 2023-07-03 00:50_

---
