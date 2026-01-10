```yaml
number: 2902
title: Rename rules according to naming convention
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2023-02-14T21:40:48Z
updated_at: 2023-03-22T15:36:03Z
url: https://github.com/astral-sh/ruff/issues/2902
synced_at: 2026-01-10T11:09:45Z
```

# Rename rules according to naming convention

---

_Issue opened by @not-my-profile on 2023-02-14 21:40_

This is the tracking issue for our effort to rename our rules according to our [rule naming convention](https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention).

* [ ] Rule names shouldn't start with `use-*`.
* [ ] Rule names shouldn't start with `consider-*`.
* [ ] `pathlib-` to `os-path-` will be done in #2348

Some related renaming efforts:

* [x] We want to prefix `pytest-*` to all pytest-style rule names.
* [x] We want to prefix `pandas-` to all pandas-vet rule names.

See also #2714.

---

_Comment by @charliermarsh on 2023-02-14 23:43_

I also added `numpy-*` to the NumPy-specific rules.

---

_Comment by @charliermarsh on 2023-02-26 20:20_

I'm gonna try and do a pass on _all_ rules and propose the changes here.

---

_Closed by @charliermarsh on 2023-03-22 15:36_

---
