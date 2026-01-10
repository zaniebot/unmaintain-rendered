```yaml
number: 1025
title: "numpy percentile \"It doesn't have an __iter__ method or a __getitem__ method info: rule not-iterable is enabled by default\""
type: issue
state: closed
author: Gabriel-p
labels: []
assignees: []
created_at: 2025-08-17T12:32:11Z
updated_at: 2025-08-17T12:38:11Z
url: https://github.com/astral-sh/ty/issues/1025
synced_at: 2026-01-10T02:06:24Z
```

# numpy percentile "It doesn't have an __iter__ method or a __getitem__ method info: rule not-iterable is enabled by default"

---

_Issue opened by @Gabriel-p on 2025-08-17 12:32_

### Summary

Running the code below with `uvx ty check temp.py` (brought from https://github.com/numpy/numpy/issues/29573)

### Reproduce the code example:

```python
import numpy as np

x = np.random.uniform(0, 100, 1000)

for pm in np.percentile(x, (10, 20, 30)):
    print(pm)

for pm in np.nanpercentile(x, (10, 20, 30)):
    print(pm)

p_75, p_85, p_95 = np.percentile(x, (75, 85, 95))

dim_x = np.percentile(x, (1, 99))
print(dim_x[0])
```

### Error message:

```shell
error[not-iterable]: Object of type `floating[Any]` is not iterable
 --> temp.py:6:11
  |
4 | x = np.random.uniform(0, 100, 1000)
5 |
6 | for pm in np.percentile(x, (10, 20, 30)):
  |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
7 |     print(pm)
  |
info: It doesn't have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default

error[not-iterable]: Object of type `floating[Any]` is not iterable
  --> temp.py:9:11
   |
 7 |     print(pm)
 8 |
 9 | for pm in np.nanpercentile(x, (10, 20, 30)):
   |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
10 |     print(pm)
   |
info: It doesn't have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default

error[not-iterable]: Object of type `floating[Any]` is not iterable
  --> temp.py:12:20
   |
10 |     print(pm)
11 |
12 | p_75, p_85, p_95 = np.percentile(x, (75, 85, 95))
   |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: It doesn't have an `__iter__` method or a `__getitem__` method
info: rule `not-iterable` is enabled by default

error[non-subscriptable]: Cannot subscript object of type `floating[Any]` with no `__getitem__` method
  --> temp.py:16:7
   |
15 | dim_x = np.percentile(x, (1, 99))
16 | print(dim_x[0])
   |       ^^^^^
   |
info: rule `non-subscriptable` is enabled by default
```

### Python and NumPy Versions:

2.2.4
3.11.11 (main, Feb  5 2025, 19:11:07) [Clang 19.1.6 ]

### Version

ty 0.0.1-alpha.18

---

_Comment by @AlexWaygood on 2025-08-17 12:38_

Thanks for the report! The reason why we're inferring the wrong thing here is that numpy is using overloads where the arguments are annotated using type aliases. We're picking the wrong overload because we have only very limited support for type aliases right now. TL;DR, this is the same issue as https://github.com/astral-sh/ty/issues/221 :-)

---

_Closed by @AlexWaygood on 2025-08-17 12:38_

---
