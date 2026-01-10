```yaml
number: 15245
title: Strange fix behavior for UP006/NonPEP585Annotation fixer since ruff 0.8.5
type: issue
state: open
author: oprypin
labels:
  - fixes
assignees: []
created_at: 2025-01-03T23:38:10Z
updated_at: 2025-01-04T15:53:30Z
url: https://github.com/astral-sh/ruff/issues/15245
synced_at: 2026-01-10T11:09:56Z
```

# Strange fix behavior for UP006/NonPEP585Annotation fixer since ruff 0.8.5

---

_Issue opened by @oprypin on 2025-01-03 23:38_

I am reporting strange behaviors since ruff 0.8.5. I have not tried ruff from master branch.

# Example 1

Example file `test.py`:

```python
import collections
import collections.abc
from typing import Callable, Iterable


def test(a: Iterable[str], b: Callable[[str], None]):
    return collections.ChainMap()
```

Command:
```bash
ruff --isolated check --target-version=py39 --select=UP006,UP035 --preview --fix --unsafe-fixes test.py
```

* File content after ruff 0.8.4 applied:
  (doesn't fix `Callable` because it's [marked as py310+](https://github.com/astral-sh/ruff/issues/15246#issuecomment-2569970746))

  ```python
  import collections
  import collections.abc
  from typing import Callable
  from collections.abc import Iterable


  def test(a: Iterable[str], b: Callable[[str], None]):
      return collections.ChainMap()
  ```

* File content after ruff 0.8.5 applied:
  (weird!! explicit reference to `collections.abc.Callable`)

  ```python
  import collections
  import collections.abc
  from typing import Callable
  from collections.abc import Iterable


  def test(a: Iterable[str], b: collections.abc.Callable[[str], None]):
      return collections.ChainMap()
  ```


---

# Example 2

Example file `test.py`:

```python
from typing import Callable, Iterable


def test(a: Iterable[str], b: Callable[[str], None]):
    pass
```

Command:
```bash
ruff --isolated check --target-version=py39 --select=UP006,UP035 --preview --fix --unsafe-fixes test.py
```

* File content after ruff 0.8.4 applied:

  ```python
  from typing import Callable
  from collections.abc import Iterable
  
  
  def test(a: Iterable[str], b: Callable[[str], None]):
      pass
  ```

* File content after ruff 0.8.5 applied:

  Same, but with this additional output:

  ```
  error: Failed to create fix for NonPEP585Annotation: Unable to insert `Iterable` into scope due to name conflict
  error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
  error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
  test.py:5:31: UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
    |
  5 | def test(a: Iterable[str], b: Callable[[str], None]):
    |                               ^^^^^^^^ UP006
  6 |     pass
    |
    = help: Replace with `collections.abc.Callable`

  Found 2 errors (1 fixed, 1 remaining).
  ```

  This was referenced in #15229
  but the description of the "fix" for that issue seems to imply that the error messages themselves are the only problem, which I think is not right.
  Ruff should be able to replace `from typing import Callable`.

---

# Example 3

Example file `test.py`:

```python
from __future__ import annotations

from typing import Callable, Iterable


def test(a: Iterable[str], b: Callable[[str], None]):
    pass
```

Command: (note `py38` here, unlike all other examples)
```bash
ruff --isolated check --target-version=py38 --select=UP006,UP035 --preview --fix --unsafe-fixes test.py
```

* File content after ruff 0.8.4 applied:

   No change, no errors

* File content after ruff 0.8.5 applied:

   No change, but with this additional output (why does it fail to perform the fixes here??):

  ```
  error: Failed to create fix for NonPEP585Annotation: Unable to insert `Iterable` into scope due to name conflict
  error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
  test.py:6:13: UP006 Use `collections.abc.Iterable` instead of `Iterable` for type annotation
    |
  6 | def test(a: Iterable[str], b: Callable[[str], None]):
    |             ^^^^^^^^ UP006
  7 |     return collections.ChainMap()
    |
    = help: Replace with `collections.abc.Iterable`
  
  test.py:6:31: UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
    |
  6 | def test(a: Iterable[str], b: Callable[[str], None]):
    |                               ^^^^^^^^ UP006
  7 |     return collections.ChainMap()
    |
    = help: Replace with `collections.abc.Callable`
  
  Found 2 errors.
  ```

# Example 4

Example file `test.py`:

```python
from __future__ import annotations

import collections
import collections.abc
from typing import Callable, Iterable


def test(a: Iterable[str], b: Callable[[str], None]):
    return collections.ChainMap()
```

Command:

```bash
ruff --isolated check --target-version=py39 --select=UP006,UP035 --preview --fix --unsafe-fixes test.py
```

* File content after ruff 0.8.4 applied:

  ```python
  from __future__ import annotations
  
  import collections
  import collections.abc
  from typing import Callable
  from collections.abc import Iterable
  
  
  def test(a: Iterable[str], b: Callable[[str], None]):
      return collections.ChainMap()
  ```

* File content after ruff 0.8.5 applied:
  (weirdest one yet!!! changes only one of the two imports, and then proceeds to not even use the imports!!)

  ```python
  from __future__ import annotations

  import collections
  import collections.abc
  from typing import Callable
  from collections.abc import Iterable


  def test(a: collections.abc.Iterable[str], b: collections.abc.Callable[[str], None]):
      return collections.ChainMap()
  ```

---

_Comment by @charliermarsh on 2025-01-04 02:53_

Thanks for these. At least for Example 1, I think Ruff is avoiding introducing a new import since `collections.abc` is already in the namespace. I think that one at least is... arguably correct?

---

_Comment by @dylwil3 on 2025-01-04 03:11_

This is super helpful, thank you! And apologies for the strange behavior here.

I think the two main things going on are:

First, I wonder if the autofix should first try to remove the bad import so that there's less likely to be a name collision when adding the new one.

Second, we can probably be cleverer in the implementation of the helper function that gets imports, and try to search for the most concise import to use if we have both, say, `import collections.abc` and `from collections.abc import ...` present.

---

_Label `fixes` added by @dylwil3 on 2025-01-04 03:12_

---

_Comment by @oprypin on 2025-01-04 11:08_

For Example 1, Ruff 0.8.4 is doing what I would expect, and 0.8.5 does not.
Let's switch to `py310` for posterity:

```console
$ ruff --isolated check --target-version=py310 --select=UP006,UP035,F401 --preview --fix --unsafe-fixes test.py
```
 
* File content after ruff 0.8.4 applied:
  ```python
  import collections
  import collections.abc
  from typing import Callable, Iterable
  
  
  def test(a: Iterable[str], b: Callable[[str], None]):
      return collections.ChainMap()
  ```
* File content after ruff 0.8.5 applied:
  ```python
  import collections
  import collections.abc
  
  
  def test(a: collections.abc.Iterable[str], b: collections.abc.Callable[[str], None]):
    return collections.ChainMap()
  ```

Well, in **all** of the above cases, Ruff 0.8.4 is doing what I would expect, I just didn't realize why there was a difference with `Callable` at first, but now it makes sense, and at `py310` everything looks perfect to me.

---

I think the fixer should not care whatsoever about an existing import of `collections.abc` and simply use a direct import.

Consider this related example:

```python
import typing

from typing_extensions import Final

a: Final[str] = typing.cast(str, 123)
```

It simply replaces `from typing_extensions import Final` with `from typing import Final`, it doesn't care that there's a `typing` import. (and I guess this works correctly because it's the  `UP035` fixer kicking in, not `UP006`, as it's not covered.)

It is my opinion that the fixer should treat additions of `from collections.abc` imports exactly the same as the additions of `from typing` imports are treated.

---

_Comment by @MichaReiser on 2025-01-04 12:20_

Thanks. I reverted the change and published a bugfix release 0.8.6

---

_Comment by @oprypin on 2025-01-04 12:36_

Thanks a lot!!

---

_Comment by @dangotbanned on 2025-01-04 15:53_

Thanks for the fix in #15250 @MichaReiser 

I resolved these already, but linking the relevant PRs from `altair` in case they're helpful 
- https://github.com/vega/altair/pull/3736
- https://github.com/vega/altair/pull/3746

---
