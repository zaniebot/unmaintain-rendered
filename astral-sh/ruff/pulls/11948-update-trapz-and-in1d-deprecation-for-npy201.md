```yaml
number: 11948
title: "Update `trapz` and `in1d` deprecation for NPY201"
type: pull_request
state: merged
author: dedebenui
labels:
  - rule
assignees: []
merged: true
base: main
head: main
created_at: 2024-06-20T07:40:09Z
updated_at: 2024-06-21T06:08:00Z
url: https://github.com/astral-sh/ruff/pull/11948
synced_at: 2026-01-12T15:55:39Z
```

# Update `trapz` and `in1d` deprecation for NPY201

---

_@dedebenui_

## Summary

Adds missing deprecation of `trapz` and `in1d` for NPY201 as listed in the [migration guide](https://numpy.org/devdocs/numpy_2_0_migration_guide.html#changes-to-namespaces). The change from `trapz` to `trapezoid` is not backwards compatible.


---

_@MichaReiser approved on 2024-06-21 06:07_

Thanks. That makes sense to me. I first wondered if `in1d`, `row_stack`, and `trapz` were intentionally left out because they will only be removed after the 2.0 release but `row_stack` is already on the list, so that can't be the reason. 

Thank you

---

_Label `rule` added by @MichaReiser on 2024-06-21 06:07_

---

_Merged by @MichaReiser on 2024-06-21 06:08_

---

_Closed by @MichaReiser on 2024-06-21 06:08_

---
