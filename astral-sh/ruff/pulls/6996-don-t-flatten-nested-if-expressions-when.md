```yaml
number: 6996
title: "Don't \"flatten\" nested if expressions when formatting"
type: pull_request
state: merged
author: LaBatata101
labels:
  - formatter
assignees: []
merged: true
base: main
head: nested-if-expr-formatting
created_at: 2023-08-29T21:06:18Z
updated_at: 2023-12-01T23:46:17Z
url: https://github.com/astral-sh/ruff/pull/6996
synced_at: 2026-01-10T23:40:55Z
```

# Don't "flatten" nested if expressions when formatting

---

_Pull request opened by @LaBatata101 on 2023-08-29 21:06_


## Summary

Closes #6939

## Test Plan

`cargo test` 


---

_Label `formatter` added by @charliermarsh on 2023-08-29 21:23_

---

_@charliermarsh approved on 2023-08-30 04:02_

LGTM -- added some additional tests and verified against Black.

---

_Merged by @charliermarsh on 2023-08-30 04:11_

---

_Closed by @charliermarsh on 2023-08-30 04:11_

---

_@MichaReiser reviewed on 2023-08-30 07:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@conditional_expression.py.snap`:146 on 2023-08-30 07:03_

This seems to be a regression. But it seems to be correct when disabling the preview styles. 

---

_Comment by @MichaReiser on 2023-08-30 07:12_

This PR regresses formatting of 

```python
# Reduce Model instances to their primary key values
row_data = tuple(
    d._get_pk_val()
    if hasattr(d, "_get_pk_val")
    # Prevent "unhashable type: list" errors later on.
    else tuple(d)
    if isinstance(d, list)
    else d
    for d in row_data
)
if row_data and None not in row_data:
    pass
```
Black collapses the second `else` as it seems because of the comment. 

```python
# Reduce Model instances to their primary key values
row_data = tuple(
    d._get_pk_val() if hasattr(d, "_get_pk_val")
    # Prevent "unhashable type: list" errors later on.
    else tuple(d) if isinstance(d, list) else d
    for d in row_data
)
if row_data and None not in row_data:
    pass
```

without the comment

```python
# Reduce Model instances to their primary key values
row_data = tuple(
    d._get_pk_val()
    if hasattr(d, "_get_pk_val")
    else tuple(d)
    if isinstance(d, list)
    else d
    for d in row_data
)
if row_data and None not in row_data:
    pass
```

I prefer our formatting because there's a smaller difference between with/without comment.



---

_Comment by @MichaReiser on 2023-08-30 07:13_

Thank you!

---

_Branch deleted on 2023-12-01 23:46_

---
