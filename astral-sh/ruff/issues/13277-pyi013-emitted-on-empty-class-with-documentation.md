```yaml
number: 13277
title: "`PYI013` emitted on empty class with documentation"
type: issue
state: closed
author: DimitriPapadopoulos
labels:
  - question
assignees: []
created_at: 2024-09-07T08:39:42Z
updated_at: 2024-09-07T14:43:18Z
url: https://github.com/astral-sh/ruff/issues/13277
synced_at: 2026-01-10T11:09:55Z
```

# `PYI013` emitted on empty class with documentation

---

_Issue opened by @DimitriPapadopoulos on 2024-09-07 08:39_

When an otherwise empty class contains documentation, ruff identifies it as non-empty and emits PYI013:
```console
$ cat empty.py
class ArrayArrayCodec:
    ...
$ 
$ ruff check --isolated --select=PYI013 empty.py 
All checks passed!
$ 
$ cat nonempty.py
class ArrayArrayCodec:
    """Base class."""
    ...
$ 
$ ruff check --isolated --select=PYI013 nonempty.py 
file.py:3:5: PYI013 [*] Non-empty class body must not contain `...`
  |
1 | class ArrayArrayCodec:
2 |     """Base class."""
3 |     ...
  |     ^^^ PYI013
  |
  = help: Remove unnecessary `...`

Found 1 error.
[*] 1 fixable with the `--fix` option.
$ 
```

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  "PYI013"
* A minimal code snippet that reproduces the bug.
  ```python
  class ArrayArrayCodec:
  """Base class."""
      ...
  ```
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
   `ruff check --isolated --select=PYI013 nonempty.py`
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
  None
* The current Ruff version (`ruff --version`).
  ```ruff 0.6.4```


---

_Comment by @AlexWaygood on 2024-09-07 12:33_

Hi, thanks for the issue! This behaviour looks correct to me, however. The ellipsis in your class body is redundant; your class could be written more simply as:

```py
class ArrayArrayCodec:
   """Base class."""
```

---

_Label `question` added by @AlexWaygood on 2024-09-07 12:33_

---

_Comment by @DimitriPapadopoulos on 2024-09-07 14:42_

It's true that the Python interpreter. accepts the docstring as actual class content and `...` is therefore not needed. I guess I'll have to disable that rule as maintainers seem to prefer explicit `...` even with a docstring.

Thank you for your hard and excellent work by the way.

---

_Closed by @DimitriPapadopoulos on 2024-09-07 14:42_

---
