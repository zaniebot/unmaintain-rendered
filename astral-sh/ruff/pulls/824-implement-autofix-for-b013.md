```yaml
number: 824
title: Implement autofix for B013
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: autofix-B013
created_at: 2022-11-20T08:47:31Z
updated_at: 2022-11-21T04:12:06Z
url: https://github.com/astral-sh/ruff/pull/824
synced_at: 2026-01-12T05:48:45Z
```

# Implement autofix for B013

---

_Pull request opened by @harupy on 2022-11-20 08:47_

#389

---

_@harupy reviewed on 2022-11-20 08:48_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:24 on 2022-11-20 08:48_

```python
try:
    pass
except (ValueError,):
      # ^^^^^^^^^^^
      # type_'s range doesn't cover ().
    pass
```

is fixed to:

```python
try:
    pass
except (ValueError):
    pass
```

@charliermarsh Is it possible to remove unnecessary parens?

---

_@charliermarsh reviewed on 2022-11-20 15:03_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:24 on 2022-11-20 15:03_

I think it will require some improvements to the source code generator.

---

_@charliermarsh reviewed on 2022-11-20 15:13_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:24 on 2022-11-20 15:13_

I can take a look at it...

---

_@charliermarsh reviewed on 2022-11-20 23:20_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:24 on 2022-11-20 23:20_

Oh, it looks like the issue here is actually in the RustPython locations, which exclude the parens.

---

_Merged by @charliermarsh on 2022-11-20 23:49_

---

_Closed by @charliermarsh on 2022-11-20 23:49_

---

_Review comment by @harupy on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:52 on 2022-11-21 03:54_

@charliermarsh Nice! Can we use this approach to find the range of a class name?

```python
class MyClass(...):
      ^^^^^^^
      This
```

---

_@harupy reviewed on 2022-11-21 03:54_

---

_Review comment by @charliermarsh on `src/flake8_bugbear/plugins/redundant_tuple_in_exception_handler.rs`:52 on 2022-11-21 03:58_

Yeah probably!

I'm also wondering if `Ident` fields like the `ClassDef` name should have locations added to them in the parser...

---

_@charliermarsh reviewed on 2022-11-21 03:58_

---

_Branch deleted on 2022-11-21 04:12_

---
