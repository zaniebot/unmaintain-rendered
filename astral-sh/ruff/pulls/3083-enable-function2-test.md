```yaml
number: 3083
title: "Enable `function2` test"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/formatter-tests
created_at: 2023-02-21T04:32:16Z
updated_at: 2023-02-21T04:37:52Z
url: https://github.com/astral-sh/ruff/pull/3083
synced_at: 2026-01-12T15:55:12Z
```

# Enable `function2` test

---

_@charliermarsh_

This is another case in which we have just a minor deviation, and I want to check it in anyway while making a note of it (until we improve the snapshot setup):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Differences ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__simple_cases__function2.py.snap
Snapshot: simple_cases/function2.py
Source: crates/ruff_python_formatter/src/lib.rs:103
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: adjust_quotes(formatted.print()?.as_code())
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    3     3 │ ) -> A:
    4     4 │     with cache_dir():
    5     5 │         if something:
    6     6 │             result = CliRunner().invoke(
    7       │-                black.main, [str(src1), str(src2), "--diff", "--check"]
          7 │+                black.main,
          8 │+                [str(src1), str(src2), "--diff", "--check"],
    8     9 │             )
    9    10 │     limited.append(-limited.pop())  # negate top
   10    11 │     return A(
   11    12 │         very_long_argument_name1=very_long_value_for_the_argument,
────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
```

---

_Merged by @charliermarsh on 2023-02-21 04:37_

---

_Closed by @charliermarsh on 2023-02-21 04:37_

---

_Branch deleted on 2023-02-21 04:37_

---
