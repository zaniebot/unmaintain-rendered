---
number: 15716
title: B023 False positive when there is a list comprehension with the same variable name in a loop.
type: issue
state: open
author: vivodi
labels:
  - bug
assignees: []
created_at: 2025-01-24T13:55:07Z
updated_at: 2025-09-17T17:50:56Z
url: https://github.com/astral-sh/ruff/issues/15716
synced_at: 2026-01-07T13:12:16-06:00
---

# B023 False positive when there is a list comprehension with the same variable name in a loop.

---

_Issue opened by @vivodi on 2025-01-24 13:55_

### Description

__minimal code snippet:__
```py
for _ in range(3):
    [x for x in []]
    def func():
        lambda x: x
```
__command:__
```
ruff check --isolated --select B023 demo.py
```
__log:__
```
demo.py:5:19: B023 Function definition does not bind loop variable `x`
  |
4 |     def func():
5 |         lambda x: x
  |                   ^ B023
  |

Found 1 error.
```
__ruff version:__ 0.9.2

This is NOT a duplicate of https://github.com/astral-sh/ruff/issues/7847, because `x` is only used in a list comprehension in the same loop. Ruff should not raise a false positive in this situation.

---

_Label `bug` added by @MichaReiser on 2025-01-24 14:27_

---

_Referenced in [astral-sh/ruff#16046](../../astral-sh/ruff/issues/16046.md) on 2025-02-08 22:54_

---

_Comment by @orgua on 2025-02-28 14:40_

I got a similar false positive when variable name is not reused, but `pandas.apply()` is used:

```python
import pandas as pd

data = pd.DataFrame()
data.loc[0, "hex"] = "72756666"
data.loc[1, "hex"] = "75767576"

def modifier(value):
    return chr(int(value, 16))


for _i in range(0, 4):
    data[f"v{_i}"] = data["hex"].apply(
        lambda x: modifier(x[2 * _i : 2 * _i + 2]),
    )

print(data)
```

I am pretty sure that the code is correct and it returns what I want:

```shell
        hex v0 v1 v2 v3
0  72756666  r  u  f  f
1  75767576  u  v  u  v
```

but the linting command 

```shell
ruff check --isolated --select B023 demo.py
```

triggers

```Shell
demo.py:13:34: B023 Function definition does not bind loop variable `_i`
   |
11 | for _i in range(0, 4):
12 |     data[f"v{_i}"] = data["hex"].apply(
13 |         lambda x: modifier(x[2 * _i : 2 * _i + 2]),
   |                                  ^^ B023
14 |     )
   |

demo.py:13:43: B023 Function definition does not bind loop variable `_i`
   |
11 | for _i in range(0, 4):
12 |     data[f"v{_i}"] = data["hex"].apply(
13 |         lambda x: modifier(x[2 * _i : 2 * _i + 2]),
   |                                           ^^ B023
14 |     )
   |

Found 2 errors.
```


---

_Comment by @nyoungstudios on 2025-09-17 17:50_

I am experiencing the same issue. It is more generally any loop, not just list comprehension that causes the problem.

On ruff `0.13.0`.

Here is my example that raises a false positive.

```python
for _ in range(2):

    def add_one():
        def _add_one_inner(value):
            return value + 1

        return _add_one_inner

    for value in range(5):
        result = add_one()(value)
        print(result)
```

Here is the error from the check command.

```
B023 Function definition does not bind loop variable `value`
 --> example_error.py:5:20
  |
3 |     def add_one():
4 |         def _add_one_inner(value):
5 |             return value + 1
  |                    ^^^^^
6 |
7 |         return _add_one_inner
  |

Found 1 error.
```

And here is the output from my simple example to show this is indeed a false positive.

```
1
2
3
4
5
1
2
3
4
5
```

And to show this only happens when there is an outer loop, here is a code snippet that passes the checks which is the same except not nested in a loop.

```python
def add_one():
    def _add_one_inner(value):
        return value + 1

    return _add_one_inner

for value in range(5):
    result = add_one()(value)
    print(result)
```

---

_Referenced in [astral-sh/ruff#20507](../../astral-sh/ruff/pulls/20507.md) on 2025-09-22 05:06_

---
