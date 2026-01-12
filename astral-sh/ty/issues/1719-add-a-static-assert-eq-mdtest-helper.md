```yaml
number: 1719
title: "Add a `static_assert_eq` mdtest helper"
type: issue
state: open
author: dcreager
labels:
  - testing
  - internal
assignees: []
created_at: 2025-12-02T13:42:23Z
updated_at: 2025-12-02T16:09:09Z
url: https://github.com/astral-sh/ty/issues/1719
synced_at: 2026-01-12T15:54:25Z
```

# Add a `static_assert_eq` mdtest helper

---

_@dcreager_

We have a `static_assert` function in `ty_extensions`, which takes in a `bool` (or a value that can be coerced to a `bool`). https://github.com/astral-sh/ruff/pull/21743 recently updated several tests to use a `static_assert(actual == expected)` pattern. If that fails, all that we can show is the value that was expected to be `True`. @sharkdp suggested an analogous `static_assert_eq`, which would perform the same test, but which would be able to print out a nicer diagnostic in case the test fails.

---

_Label `internal` added by @carljm on 2025-12-02 16:09_

---

_Label `testing` added by @carljm on 2025-12-02 16:09_

---
