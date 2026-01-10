```yaml
number: 14292
title: "[Feature Request] Guard against unnecessary `int` call on functions that already return integer"
type: issue
state: closed
author: DanielYang59
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-12T05:46:44Z
updated_at: 2024-12-07T14:13:27Z
url: https://github.com/astral-sh/ruff/issues/14292
synced_at: 2026-01-10T11:09:56Z
```

# [Feature Request] Guard against unnecessary `int` call on functions that already return integer

---

_Issue opened by @DanielYang59 on 2024-11-12 05:46_

While cleaning up our code base, I noticed the following anti-patterns (?) which seem pretty well-spread:

```python
num = 0.5

# `round` with `ndigits=None` (default value) already gives int
int(round(num)) 

# I believe `int(round(num, 0))` gives the same result as `round(num)`
int(round(num, 0))

# The same applies to `len`
int(len([1, 2, 3])

# `sourcery` seems to have a rule to suggest against the following
# WARNING: `//` is the floor division, for negative results their behaviors are different
int(non_negative_number / 2)  # -> non_negative_number // 2

# Some `math` functions also give int only (perhaps there are more that I'm missing):
- math.ceil
- math.comb
- math.floor
- math.factorial
- math.gcd
- math.lcm

int(math.ceil(num))

```
---

Also calling `int` on `round(num, ndigits)` (with `ndigits` not as `None`) in general might be a bad idea as results may very likely be suprising:
```python
round(2.994, 2)  # -> 2.99
round(2.995, 2)  # -> 3.0

int(round(2.994, 2))  # -> 2
int(round(2.995, 2))  # -> 3

round(2.994)  # -> 3
round(2.995)  # -> 3
```

---

On top of the unnecessary verbosity, this also comes with performance penalty, trying to work on 10 floats for 1_000_000 repetitions:
```
Time taken for round(): 0.805204 seconds
Time taken for int(round()): 0.946426 seconds
Time taken for math.ceil(): 0.429992 seconds
Time taken for int(math.ceil()): 0.524181 seconds
```

<details>

<summary> Test script (by GPT) </summary>

```python
import timeit
import math


def round_function(value):
    return round(value)

def int_round_function(value):
    return int(round(value))

def int_ceil_function(value):
    return int(math.ceil(value))

def ceil_function(value):
    return math.ceil(value)

# Set up the test data
test_values = [0.1, 1.5, 2.3, 3.7, 4.8, 5.9999, -1.5, -2.3, -3.7, -4.8]

# Number of repetitions for the benchmarking
repetitions = 1_000_000

# Benchmarking round()
time_round = timeit.timeit(
    stmt='for v in test_values: round_function(v)',
    setup='from __main__ import round_function, test_values',
    number=repetitions
)

# Benchmarking int(round())
time_int_round = timeit.timeit(
    stmt='for v in test_values: int_round_function(v)',
    setup='from __main__ import int_round_function, test_values',
    number=repetitions
)

# Benchmarking int(math.ceil())
time_int_ceil = timeit.timeit(
    stmt='for v in test_values: int_ceil_function(v)',
    setup='from __main__ import int_ceil_function, test_values',
    number=repetitions
)

# Benchmarking math.ceil()
time_ceil = timeit.timeit(
    stmt='for v in test_values: ceil_function(v)',
    setup='from __main__ import ceil_function, test_values',
    number=repetitions
)

print(f'Time taken for round(): {time_round:.6f} seconds')
print(f'Time taken for int(round()): {time_int_round:.6f} seconds')
print(f'Time taken for math.ceil(): {time_ceil:.6f} seconds')
print(f'Time taken for int(math.ceil()): {time_int_ceil:.6f} seconds')

```


</details>

---

_Label `rule` added by @dylwil3 on 2024-11-12 12:31_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-12 12:32_

---

_Comment by @MichaReiser on 2024-11-13 08:59_

Thanks for the suggestion and the detailed write up! this makes sense to me. I merge this into https://github.com/astral-sh/ruff/issues/11412

---

_Closed by @MichaReiser on 2024-11-13 08:59_

---

_Comment by @DanielYang59 on 2024-12-07 13:53_

Thanks a ton @MichaReiser and sorry I missed this somehow

---

_Renamed from "[Feature Request] Guard against unnecessary `int` call on functions that already yield integer" to "[Feature Request] Guard against unnecessary `int` call on functions that already return integer" by @DanielYang59 on 2024-12-07 14:13_

---
