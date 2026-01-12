```yaml
number: 12780
title: F401 false positive when importing in a type-checking block
type: issue
state: closed
author: alexandermalyga
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-08-09T10:36:14Z
updated_at: 2025-02-01T17:06:32Z
url: https://github.com/astral-sh/ruff/issues/12780
synced_at: 2026-01-12T15:54:52Z
```

# F401 false positive when importing in a type-checking block

---

_@alexandermalyga_

A false positive `F401` error is raised when the import is in a type-checking block and used as a generic parameter.

foo.py
```python
class Foo: ...
```

bar.py
```python
class Bar[T]: ...
```

baz.py
```python
from bar import Bar
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from foo import Foo

class Baz(Bar["Foo"]): ...
```

raises

```
baz.py:5:21: F401 [*] `foo.Foo` imported but unused
  |
4 | if TYPE_CHECKING:
5 |     from foo import Foo
  |                     ^^^ F401
6 | 
7 | class Baz(Bar["Foo"]):
  |
  = help: Remove unused import: `foo.Foo`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Command and Ruff version:

`ruff check baz.py --isolated`
Version 0.5.7

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 12:53_

---

_Comment by @charliermarsh on 2024-08-09 13:18_

We don't treat arbitrary subscripts as generics, since they aren't known to be generics at present (i.e., that could be a dictionary access). But you can mark `bar.Bar` as a generic via [`extend-generics`](https://docs.astral.sh/ruff/settings/#lint_pyflakes_extend-generics):

```toml
[tool.ruff.lint.pyflakes]
extend-generics = ["bar.Bar"]
```

---

_Label `bug` added by @charliermarsh on 2024-08-09 13:18_

---

_Label `type-inference` added by @charliermarsh on 2024-08-09 13:18_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-09 13:18_

---

_Comment by @AlexWaygood on 2025-02-01 17:06_

Thanks for the report! Closing as a duplicate of #9298, however :-)

---

_Closed by @AlexWaygood on 2025-02-01 17:06_

---
