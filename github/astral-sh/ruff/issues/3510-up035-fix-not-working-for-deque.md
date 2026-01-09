---
number: 3510
title: UP035 --fix not working for Deque
type: issue
state: closed
author: torarvid
labels:
  - fixes
assignees: []
created_at: 2023-03-14T13:22:04Z
updated_at: 2023-05-15T22:33:27Z
url: https://github.com/astral-sh/ruff/issues/3510
synced_at: 2026-01-07T13:12:14-06:00
---

# UP035 --fix not working for Deque

---

_Issue opened by @torarvid on 2023-03-14 13:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When running `ruff --fix test.py` (Ruff 0.0.255), it doesn't change the `Deque` to `deque`
```python
from typing import Deque

a: Deque[str]
```

However, interestingly, if we replace `Deque` with `List`, it does change to `list`
```python
from typing import List

a: List[str]
```

---

_Comment by @JonathanPlasse on 2023-03-14 14:53_

- `deque` is not a builtin like `list`. `deque` need to be imported. Currently, it is not possible to auto-fix. We first need #835.

---

_Label `autofix` added by @charliermarsh on 2023-03-14 14:58_

---

_Comment by @charliermarsh on 2023-03-14 14:59_

Yeah we can't fix this right now -- but, in theory, we should be able to.

---

_Comment by @torarvid on 2023-03-15 07:47_

Ah, I didn't know about #835, but I understand this issue depends on that. Thanks for the info 👍🏻 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-13 17:43_

---

_Referenced in [astral-sh/ruff#4420](../../astral-sh/ruff/pulls/4420.md) on 2023-05-13 19:42_

---

_Comment by @tmke8 on 2023-05-14 15:08_

This is the full list of `typing` imports that can be rewritten on Python 3.9:

- `collections.deque`  # typing.Deque
- `collections.defaultdict`  # typing.DefaultDict
- `collections.OrderedDict`
- `collections.Counter`
- `collections.ChainMap`
- `collections.abc.Awaitable`
- `collections.abc.Coroutine`
- `collections.abc.AsyncIterable`
- `collections.abc.AsyncIterator`
- `collections.abc.AsyncGenerator`
- `collections.abc.Iterable`
- `collections.abc.Iterator`
- `collections.abc.Generator`
- `collections.abc.Reversible`
- `collections.abc.Container`
- `collections.abc.Collection`
- `collections.abc.Callable`
- `collections.abc.Set` # typing.AbstractSet
- `collections.abc.MutableSet`
- `collections.abc.Mapping`
- `collections.abc.MutableMapping`
- `collections.abc.Sequence`
- `collections.abc.MutableSequence`
- `collections.abc.ByteString`
- `collections.abc.MappingView`
- `collections.abc.KeysView`
- `collections.abc.ItemsView`
- `collections.abc.ValuesView`
- `contextlib.AbstractContextManager` # typing.ContextManager
- `contextlib.AbstractAsyncContextManager` # typing.AsyncContextManager
- `re.Pattern` # typing.Pattern, typing.re.Pattern
- `re.Match` # typing.Match, typing.re.Match

source: https://peps.python.org/pep-0585/#implementation

---

_Comment by @charliermarsh on 2023-05-14 22:34_

The rewrites that don't require _renames_ are already handled by `UP035` (e.g., importing `from collections import Counter` instead of `from typing import Counter`).

---

_Closed by @charliermarsh on 2023-05-15 22:33_

---
