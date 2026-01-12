```yaml
number: 2430
title: more builtin name checks when autofixing
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: 2427-sim-bool-builtin
created_at: 2023-02-01T00:27:32Z
updated_at: 2023-02-01T13:16:48Z
url: https://github.com/astral-sh/ruff/pull/2430
synced_at: 2026-01-12T04:52:00Z
```

# more builtin name checks when autofixing

---

_Pull request opened by @spaceone on 2023-02-01 00:27_

Issue #2427

on test is failing and I cannot explain it. Maybe `checker.is_builtin()` returns the wrong result here as it checks outside of the function?

---

_Comment by @charliermarsh on 2023-02-01 04:35_

Which test is failing? Can you link it?

---

_Comment by @spaceone on 2023-02-01 09:31_

[the test link](https://github.com/charliermarsh/ruff/actions/runs/4059664017/jobs/6987962479) probably won't help but the output of `cargo insta review`
```
Reviewing [1/1] ruff@0.0.239:
Snapshot file: src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM110_SIM110.py.snap
Snapshot: rules__flake8_simplify__tests__SIM110_SIM110
Source: src/rules/flake8_simplify/mod.rs:48
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: diagnostics
─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   53    53 │     end_location:
   54    54 │       row: 77
   55    55 │       column: 20
   56    56 │   parent: ~
         57 │+- kind:
         58 │+    ConvertLoopToAny:
         59 │+      any: return any(check(x) for x in iterable)
         60 │+  location:
         61 │+    row: 124
         62 │+    column: 4
         63 │+  end_location:
         64 │+    row: 126
         65 │+    column: 23
         66 │+  fix:
         67 │+    content:
         68 │+      - return any(check(x) for x in iterable)
         69 │+    location:
         70 │+      row: 124
         71 │+      column: 4
         72 │+    end_location:
         73 │+      row: 127
         74 │+      column: 16
         75 │+  parent: ~

```

there is a diff for the snapshot which shouldn't be there because ruff should leave that code as is.

---

_Comment by @charliermarsh on 2023-02-01 12:47_

Ah yeah this failure makes sense but it's a little complicated. It has to do with the way we iterate over the body for those _specific_ rules. I'm going to adjust it, and then we can rebase this.

---

_Merged by @charliermarsh on 2023-02-01 13:16_

---

_Closed by @charliermarsh on 2023-02-01 13:16_

---
