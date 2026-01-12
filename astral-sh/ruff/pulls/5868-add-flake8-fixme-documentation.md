```yaml
number: 5868
title: "Add `flake8-fixme` documentation"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: fixme-docs
created_at: 2023-07-18T17:31:23Z
updated_at: 2023-07-20T11:10:18Z
url: https://github.com/astral-sh/ruff/pull/5868
synced_at: 2026-01-12T15:55:19Z
```

# Add `flake8-fixme` documentation

---

_@tjkuson_

## Summary

Completes documentation for the `flake8-fixme` (`FIX`) ruleset. Related to #2646.

Tweaks the violation message. For example,

```
FIX001 Line contains FIXME
```

becomes

```
FIX001 Line contains FIXME, consider resolving the issue
```

This is because the previous message was unclear if it was warning against the use of FIXME tags per se, or the code the FIXME tag was annotating.


## Test Plan

`cargo test && python scripts/check_docs_formatted.py`


---

_Label `documentation` added by @charliermarsh on 2023-07-20 02:08_

---

_Merged by @charliermarsh on 2023-07-20 02:21_

---

_Closed by @charliermarsh on 2023-07-20 02:21_

---

_Branch deleted on 2023-07-20 11:10_

---
