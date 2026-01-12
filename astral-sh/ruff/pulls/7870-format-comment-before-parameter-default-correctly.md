```yaml
number: 7870
title: Format comment before parameter default correctly
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-7603
created_at: 2023-10-09T13:41:28Z
updated_at: 2023-10-12T15:50:14Z
url: https://github.com/astral-sh/ruff/pull/7870
synced_at: 2026-01-12T02:32:41Z
```

# Format comment before parameter default correctly

---

_Pull request opened by @konstin on 2023-10-09 13:41_

**Summary** Handle comment before the default values of function parameters correctly by inserting a line break instead of space after the equals sign where required.

```python
def f(
    a = # parameter trailing comment; needs line break
    1,
    b =
    # default leading comment; needs line break
    2,
    c = ( # the default leading can only be end-of-line with parentheses; no line break
        3
    ),
    d = (
        # own line leading comment with parentheses; no line break
        4
    )
)
```

Fixes #7603

**Test Plan** Added the different cases and one more complex case as fixtures.

---

_@konstin reviewed on 2023-10-09 13:42_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/parameter_with_default.rs`:64 on 2023-10-09 13:42_

This was meant as an if-then-else, not sure if it's better to split the `write!` into parts

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/other/parameter_with_default.rs`:64 on 2023-10-12 15:40_

I think it's fine as-is.

---

_@charliermarsh approved on 2023-10-12 15:40_

---

_Label `bug` added by @charliermarsh on 2023-10-12 15:41_

---

_Label `formatter` added by @charliermarsh on 2023-10-12 15:41_

---

_Merged by @konstin on 2023-10-12 15:50_

---

_Closed by @konstin on 2023-10-12 15:50_

---

_Branch deleted on 2023-10-12 15:50_

---
