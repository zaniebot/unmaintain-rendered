```yaml
number: 3080
title: "Enable `tupleassign` test"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter-tests
created_at: 2023-02-21T00:39:54Z
updated_at: 2023-02-21T00:42:24Z
url: https://github.com/astral-sh/ruff/pull/3080
synced_at: 2026-01-12T15:55:12Z
```

# Enable `tupleassign` test

---

_@charliermarsh_

This actually _does_ deviate from Black, but I don't yet understand Black's logic, so I'm enabling it anyway:

```
❯ cargo insta review
Reviewing [1/1] ruff_python_formatter@0.0.0:
Snapshot file: crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__simple_cases__tupleassign.py.snap
Snapshot: tests__simple_cases__tupleassign
Source: crates/ruff_python_formatter/src/lib.rs:85
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: formatted.print()?.as_code()
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    2     2 │     sdfjklsdfsjldkflkjsf,
    3     3 │     sdfjsdfjlksdljkfsdlkf,
    4     4 │     sdfsdjfklsdfjlksdljkf,
    5     5 │     sdsfsdfjskdflsfsdf,
    6       │-) = (1, 2, 3)
          6 │+) = 1, 2, 3
    7     7 │
    8     8 │ # This is as well.
    9     9 │ (this_will_be_wrapped_in_parens,) = struct.unpack(b"12345678901234567890")
   10    10 │
────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

```

---

_Renamed from "Enable tupleassign test" to "Enable `tupleassign` test" by @charliermarsh on 2023-02-21 00:39_

---

_Label `autoformatter` added by @charliermarsh on 2023-02-21 00:40_

---

_Merged by @charliermarsh on 2023-02-21 00:42_

---

_Closed by @charliermarsh on 2023-02-21 00:42_

---

_Branch deleted on 2023-02-21 00:42_

---
