```yaml
number: 2263
title: "Implement autofix for RET504 Unnecessary variable assignment before `return` statement"
type: issue
state: closed
author: spaceone
labels:
  - fixes
assignees: []
created_at: 2023-01-27T16:07:55Z
updated_at: 2023-06-10T04:32:05Z
url: https://github.com/astral-sh/ruff/issues/2263
synced_at: 2026-01-12T15:54:42Z
```

# Implement autofix for RET504 Unnecessary variable assignment before `return` statement

---

_@spaceone_

A fixer for `RET504 Unnecessary variable assignment before 'return' statement` would be nice.

```diff
 def random_zone():
-    random_zone = f'{uts.random_string()}.{uts.random_string()}'
-    return random_zone
+    return f'{uts.random_string()}.{uts.random_string()}'
```

At least for this simple case I imagine this easy to implement?

---

_Label `autofix` added by @charliermarsh on 2023-01-27 16:19_

---

_Renamed from "fixer for RET504 Unnecessary variable assignment before `return` statement" to "Implement autofix for RET504 Unnecessary variable assignment before `return` statement" by @spaceone on 2023-01-31 10:39_

---

_Comment by @Sawbez on 2023-02-05 18:28_

I can work on this today.

---

_Comment by @charliermarsh on 2023-02-05 20:02_

Go for it!

---

_Closed by @charliermarsh on 2023-06-10 04:32_

---
