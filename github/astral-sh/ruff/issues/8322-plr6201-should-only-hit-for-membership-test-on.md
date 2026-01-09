---
number: 8322
title: PLR6201 should only hit for membership test on constant collections
type: issue
state: open
author: LefterisJP
labels:
  - rule
assignees: []
created_at: 2023-10-29T08:22:45Z
updated_at: 2025-01-28T19:13:24Z
url: https://github.com/astral-sh/ruff/issues/8322
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR6201 should only hit for membership test on constant collections

---

_Issue opened by @LefterisJP on 2023-10-29 08:22_

### Ruff version

ruff 0.1.3

### Ruff settings

Just make sure PLR6201 is enabled

### Problem

```python
a = 1
b = 2
x = 5

test1 = 2 in [1, 2, 3]
test2 = 'c' in ('a', 'b', 'c')
test3 = x in (a, b)
```

Run ruff on this one.

It will hit you with something like:

```
test4.py:5:14: PLR6201 Use a `set` literal when testing for membership
test4.py:6:16: PLR6201 Use a `set` literal when testing for membership
test4.py:7:14: PLR6201 Use a `set` literal when testing for membership
Found 3 errors.
No fixes available (3 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

As far as I understand this rule should only hit us for **constant** collections as the optimization in question mentions: https://docs.python.org/3/whatsnew/3.2.html#optimizations . So I would not expect to see it in test3.

For collections that have variables inside the optimizer does not, or more like can not do anything so initializing a set does not help in any way and incurs the set initialization "penalty" which the optimizer fixes for constant values.

Does this make sense?

---

_Comment by @LefterisJP on 2023-10-29 10:06_

I also just interalized after writing this that in Python constants for the interpreter is just literals. Anything else, even if say you use the typing `Final` attribute won't be counted as constant in any way. Even Enum values are not considered constant. So this optimization happens in a much smaller amount of cases than I originally thought. Which is sad.

And there is a difference between using set and tuple and list for performance. OR more like between set and tuple/list. The latter are almost identical in some quick tests.

```python
import dis
import timeit
from enum import Enum, auto
from typing import Final


class Color(Enum):
    RED = auto()
    BLUE = auto()
    GREEN = auto()
    WHITE = auto()
    YELLOW = auto()
    BB = auto()
    CC = auto()
    DD = auto()
    FF = auto()
    EE = auto()

a: Final = 1
b: Final = 2

def foo():
    test1 = 2 in [1, 2, 3]
    test2 = 'c' in {'a', 'b', 'c'}
    test3 = b in {Color.RED, Color.GREEN}
    return None


dis.dis(foo)

def with_set():
    return b in {Color.RED, Color.BLUE, Color.GREEN, Color.WHITE, Color.YELLOW, Color.BB, Color.CC, Color.DD, Color.FF, Color.EE}

def with_tuple():
    return b in (Color.RED, Color.BLUE, Color.GREEN, Color.WHITE, Color.YELLOW, Color.BB, Color.CC, Color.DD, Color.FF, Color.EE)

def with_list():
    return b in [Color.RED, Color.BLUE, Color.GREEN, Color.WHITE, Color.YELLOW, Color.BB, Color.CC, Color.DD, Color.FF, Color.EE]


print(f'With set: {timeit.timeit(with_set, number=10000)}')
print(f'With tuple: {timeit.timeit(with_tuple, number=10000)}')
print(f'With list: {timeit.timeit(with_list, number=10000)}')
```

Output:

```
 23           0 LOAD_CONST               1 (2)
              2 LOAD_CONST               2 ((1, 2, 3))
              4 CONTAINS_OP              0
              6 STORE_FAST               0 (test1)

 24           8 LOAD_CONST               3 ('c')
             10 LOAD_CONST               4 (frozenset({'a', 'c', 'b'}))
             12 CONTAINS_OP              0
             14 STORE_FAST               1 (test2)

 25          16 LOAD_GLOBAL              0 (b)
             18 LOAD_GLOBAL              1 (Color)
             20 LOAD_ATTR                2 (RED)
             22 LOAD_GLOBAL              1 (Color)
             24 LOAD_ATTR                3 (GREEN)
             26 BUILD_SET                2
             28 CONTAINS_OP              0
             30 STORE_FAST               2 (test3)

 26          32 LOAD_CONST               0 (None)
             34 RETURN_VALUE
With set: 0.011320679999698768
With tuple: 0.005961606000710162
With list: 0.0059832759998244
```

---

_Referenced in [rotki/rotki#6849](../../rotki/rotki/pulls/6849.md) on 2023-10-29 11:29_

---

_Label `rule` added by @charliermarsh on 2023-10-30 23:31_

---

_Referenced in [astral-sh/ruff#8758](../../astral-sh/ruff/issues/8758.md) on 2023-11-20 12:12_

---

_Referenced in [astral-sh/ruff#12051](../../astral-sh/ruff/pulls/12051.md) on 2024-06-27 09:53_

---

_Comment by @jogo-openai on 2025-01-28 19:08_

Example where this rule is unsafe:

```python
import numpy as np

arr = np.array([1, 2, 3], dtype=np.float32)

print(arr.dtype in (np.float32, np.float16))
print(arr.dtype in {np.float32, np.float16})
```

```
True
False
```

On the numpy side this seems related to https://github.com/numpy/numpy/issues/7242 

---
