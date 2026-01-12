```yaml
number: 5062
title: Add support for PEP 695
type: issue
state: closed
author: charliermarsh
labels:
  - core
  - python312
assignees: []
created_at: 2023-06-13T18:59:12Z
updated_at: 2023-08-28T15:01:44Z
url: https://github.com/astral-sh/ruff/issues/5062
synced_at: 2026-01-12T15:54:45Z
```

# Add support for PEP 695

---

_@charliermarsh_

## Summary

[PEP 695](https://peps.python.org/pep-0695/) adds new syntax for type parameters and generics. A few examples:

```py
class ClassA[T: str]:
    def method1(self) -> T:
        ...

type ListOrSet[T] = list[T] | set[T]
```

There are a few pieces of work involved in this:

- Adding these elements to the AST.
- Adding these elements to the parser (especially the `type` soft keyword).
- Adding these elements to the visitor, and to any rules that do custom visiting.
- Adding these elements to our semantic model (need to look into how scoping and resolution works).

This will be Ruff's first time undergoing a significant grammar and syntax change (apart from adding structural pattern matching), so it's a great opportunity to set the project's standards for keeping up with Python's evolution.

Specifically, the final Python 3.12 release is expected on 2023-10-02. We should aim to support PEP 695 by that date _at the latest_.


---

_Label `core` added by @charliermarsh on 2023-06-13 18:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-13 18:59_

---

_Comment by @charliermarsh on 2023-06-13 18:59_

I will take ownership of this work.

---

_Assigned to @zanieb by @charliermarsh on 2023-07-14 05:28_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-07-14 05:28_

---

_Comment by @charliermarsh on 2023-07-14 05:28_

\cc @zanieb who's working on this :)

---

_Comment by @zanieb on 2023-07-14 13:56_

Progress on the Rust Python parser can be tracked at https://github.com/RustPython/Parser/issues/82 

---

_Comment by @zanieb on 2023-07-31 15:36_

We now support parsing and analysis (#6109) of type aliases and parameters. 

Formatting support is being added in #6161 and #6162.

---

_Closed by @zanieb on 2023-08-02 23:46_

---

_Label `python312` added by @zanieb on 2023-08-24 20:02_

---

_Comment by @zanieb on 2023-08-28 15:01_

Additional resource; may be useful for refinements to scoping https://jellezijlstra.github.io/pep695

---
