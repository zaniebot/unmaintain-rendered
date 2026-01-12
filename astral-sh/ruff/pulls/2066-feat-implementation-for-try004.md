```yaml
number: 2066
title: "feat: implementation for TRY004"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: feat/tryceratops-try004
created_at: 2023-01-21T18:33:34Z
updated_at: 2023-01-21T19:59:00Z
url: https://github.com/astral-sh/ruff/pull/2066
synced_at: 2026-01-12T15:55:07Z
```

# feat: implementation for TRY004

---

_@sbrugman_

https://github.com/charliermarsh/ruff/issues/2056

---

_@charliermarsh reviewed on 2023-01-21 18:40_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/prefer_type_error.rs`:95 on 2023-01-21 18:40_

I think it'd be preferable to instead do:

```rs
checker.resolve_call_path(exc).map_or(false, |call_path| 
    [
        "ArithmeticError",
        "AssertionError",
        "AttributeError",
        "BufferError",
        "EOFError",
        "Exception",
        "ImportError",
        "LookupError",
        "MemoryError",
        "NameError",
        "ReferenceError",
        "RuntimeError",
        "SyntaxError",
        "SystemError",
        "ValueError",
    ].iter().any(|target| call_path.as_slice() == ["", target]))
```

That will also be robust to overridden builtins -- so if a user overrides `ValueError`, this won't be truth-y.


---

_@sbrugman reviewed on 2023-01-21 19:11_

---

_Review comment by @sbrugman on `src/rules/tryceratops/rules/prefer_type_error.rs`:95 on 2023-01-21 19:11_

Thanks! Incorporated this suggestion

---

_@charliermarsh reviewed on 2023-01-21 19:17_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/prefer_type_error.rs`:95 on 2023-01-21 19:17_

(`""` as the first segment in the path is a weird convention we have to indicate built-in.)

---

_Merged by @charliermarsh on 2023-01-21 19:59_

---

_Closed by @charliermarsh on 2023-01-21 19:59_

---
