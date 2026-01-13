```yaml
number: 22550
title: Correct comment about public functions starting with an underscore.
type: pull_request
state: merged
author: manueljacob
labels:
  - internal
assignees: []
merged: true
base: main
head: comment_about_public_functions
created_at: 2026-01-13T13:44:36Z
updated_at: 2026-01-13T14:04:19Z
url: https://github.com/astral-sh/ruff/pull/22550
synced_at: 2026-01-13T14:32:13Z
```

# Correct comment about public functions starting with an underscore.

---

_@manueljacob_

Currently, the code to which the comment applies only handles `os._exit`. It is a public function, not a “documented private” one (neither in the sense of “documented to be private” nor in the sense of “documented and private”). Here are a few reasons why it should be considered a public function:

[PEP 8](https://peps.python.org/pep-0008/#public-and-internal-interfaces) says “Documented interfaces are considered public, unless the documentation explicitly declares them to be provisional or internal interfaces exempt from the usual backwards compatibility guarantees.” The documentation for `os._exit` doesn’t say that the function is non-public or internal.

[The documentation for modules’ `__all__`](https://docs.python.org/3/reference/simple_stmts.html#module.__all__) says “The names given in `__all__` are all considered public and are required to exist.”. [`os._exit` is part of `os.__all__`](https://github.com/python/cpython/blob/v3.14.2/Lib/os.py#L58). Therefore, it should be considered public.

`os._exit` was named like this because it calls the C function `_exit`, not because it’s private.

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 13:58_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@AlexWaygood approved on 2026-01-13 14:04_

---

_Label `internal` added by @AlexWaygood on 2026-01-13 14:04_

---

_Merged by @AlexWaygood on 2026-01-13 14:04_

---

_Closed by @AlexWaygood on 2026-01-13 14:04_

---
