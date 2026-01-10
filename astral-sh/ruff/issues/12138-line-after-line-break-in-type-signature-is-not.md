```yaml
number: 12138
title: Line after line break in type signature is not indented as Ruff-example in known deviations page
type: issue
state: closed
author: pjonsson
labels:
  - documentation
assignees: []
created_at: 2024-07-01T13:06:31Z
updated_at: 2024-07-03T08:03:09Z
url: https://github.com/astral-sh/ruff/issues/12138
synced_at: 2026-01-10T11:09:54Z
```

# Line after line break in type signature is not indented as Ruff-example in known deviations page

---

_Issue opened by @pjonsson on 2024-07-01 13:06_

This resembles #11791, but that talks about assignments, the example below does not have any assignments in the relevant lines. It's also possible Ruff behaves as it should, and the formatted example on the known deviations page is outdated.

This example:
```python
from collections.abc import Callable
from multiprocessing.pool import Pool as PoolClass
from pathlib import Path


class AbstractStorage:
    value: int = 3


class Processor:
    def __init__(
        self,
        process: Callable[[PoolClass, AbstractStorage, str, str, Path, Path, Path], str] | None,
    ) -> None:
        self.process = process
```
gets reformatted to:
```python
from collections.abc import Callable
from multiprocessing.pool import Pool as PoolClass
from pathlib import Path


class AbstractStorage:
    value: int = 3


class Processor:
    def __init__(
        self,
        process: Callable[[PoolClass, AbstractStorage, str, str, Path, Path, Path], str]
        | None,
    ) -> None:
        self.process = process
```
where the `|` is aligned with the start of the parameter name in the previous line. I'm using Ruff 0.5.0, but Ruff 0.4.10 behaved the same way.

The formatting of the type signature above is inconsistent with the Ruff-formatted example in https://docs.astral.sh/ruff/formatter/black/#parenthesizing-long-nested-expressions:
```python
def foo(
  i: int,
  x: Loooooooooooooooooooooooong
     | Looooooooooooooooong
     | Looooooooooooooooooooong
     | Looooooong,
  *,
  s: str,
) -> None:
  pass
```
where the `|` is aligned with the start of the type annotation of the previous line, not the start of the parameter name.

From a readability standpoint, I prefer the indentation example from the known deviations page, where a line broken type signature for a parameter is indented more than the parameter name itself.

---

_Label `formatter` added by @dhruvmanila on 2024-07-02 03:52_

---

_Label `formatter` removed by @MichaReiser on 2024-07-02 05:27_

---

_Label `documentation` added by @MichaReiser on 2024-07-02 05:27_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-07-02 05:57_

---

_Comment by @MichaReiser on 2024-07-02 05:58_

Thanks for reporting. Yes, the documentation here is incorrect. What it shows in the ruff section is *how I want it to be formatted* but isn't how ruff formats it today, unfortunately :( Let me update the documentation

---

_Closed by @MichaReiser on 2024-07-03 08:03_

---
