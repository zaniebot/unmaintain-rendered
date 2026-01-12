```yaml
number: 21314
title: "Could UP035 also catch uses of `typing.Iterable`?"
type: issue
state: closed
author: mdickinson
labels: []
assignees: []
created_at: 2025-11-07T13:51:24Z
updated_at: 2025-11-10T12:25:15Z
url: https://github.com/astral-sh/ruff/issues/21314
synced_at: 2026-01-12T15:54:57Z
```

# Could UP035 also catch uses of `typing.Iterable`?

---

_@mdickinson_

### Summary

tl;dr: UP035 catches use of the deprecated `Iterable` alias in `typing` in `from typing import Iterable; Iterable[int]`, but doesn't catch it in `import typing; typing.Iterable[int]`.

With the following Python code (stolen from the ruff tutorial), ruff extended with the python-upgrade `UP` rules complains about the use of the deprecated `Iterable` alias, and suggests replacing with `collections.abc`.

```python
# calculate.py

from typing import Iterable


def sum_even_numbers(numbers: Iterable[int]) -> int:
    """Given an iterable of integers, return the sum of all even numbers in the iterable."""
    return sum(num for num in numbers if num % 2 == 0)
```

Here are the results on my machine:

```console
~/Desktop % uvx ruff check --extend-select UP calculate.py
UP035 [*] Import from `collections.abc` instead: `Iterable`
 --> calculate.py:3:1
  |
1 | # calculate.py
2 |
3 | from typing import Iterable
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Import from `collections.abc`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

However, if I do `import typing` instead and then use `typing.Iterable` instead of `Iterable`, there's no complaint:

```python
# calculate_alt.py

import typing


def sum_even_numbers(numbers: typing.Iterable[int]) -> int:
    """Given an iterable of integers, return the sum of all even numbers in the iterable."""
    return sum(num for num in numbers if num % 2 == 0)
```

```console
~/Desktop % uvx ruff check --extend-select UP calculate_alt.py
All checks passed!
```

Is it possible for rule `UP035` (or some other rule) to complain about this second case, too?

Of course, `Iterable` is just an example here; there are several other deprecated `typing` aliases to types in `collections.abc`.

Version:

```
~/Desktop % uvx ruff --version                                
ruff 0.14.4
``` 

---

_Comment by @mdickinson on 2025-11-07 14:05_

Looks like this might be a duplicate: #3936, #7526 and #10955 are related (though #3936 is closed).

---

_Comment by @MichaReiser on 2025-11-10 12:25_

Yes, I think this is a duplicate of https://github.com/astral-sh/ruff/issues/7526 and flagging full qualified usages does make sense to me (commented on the other issue)

---

_Closed by @MichaReiser on 2025-11-10 12:25_

---
