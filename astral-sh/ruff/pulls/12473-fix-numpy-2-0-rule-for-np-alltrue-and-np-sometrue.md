```yaml
number: 12473
title: "Fix NumPy 2.0 rule for `np.alltrue` and `np.sometrue`"
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: sometrue-alltrue-fix
created_at: 2024-07-23T08:06:57Z
updated_at: 2024-07-23T08:38:37Z
url: https://github.com/astral-sh/ruff/pull/12473
synced_at: 2026-01-10T21:47:02Z
```

# Fix NumPy 2.0 rule for `np.alltrue` and `np.sometrue`

---

_Pull request opened by @mtsokol on 2024-07-23 08:06_

Fixes: https://github.com/astral-sh/ruff/issues/12416

Also to be fixed in NumPy: https://github.com/numpy/numpy/pull/27015

---

_Label `rule` added by @MichaReiser on 2024-07-23 08:13_

---

_@MichaReiser approved on 2024-07-23 08:13_

---

_Comment by @mtsokol on 2024-07-23 08:14_

There was a typo in `anytrue` name (should be `sometrue`). I will adjust the snap file. 

---

_Comment by @MichaReiser on 2024-07-23 08:31_

Thanks

---

_Merged by @MichaReiser on 2024-07-23 08:34_

---

_Closed by @MichaReiser on 2024-07-23 08:34_

---

_Branch deleted on 2024-07-23 08:38_

---
