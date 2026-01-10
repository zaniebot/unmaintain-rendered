```yaml
number: 7771
title: "[Doc] `reimplemented-starmap` does not mention performance"
type: issue
state: closed
author: Avasam
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-10-03T03:08:59Z
updated_at: 2023-10-07T13:27:03Z
url: https://github.com/astral-sh/ruff/issues/7771
synced_at: 2026-01-10T11:09:50Z
```

# [Doc] `reimplemented-starmap` does not mention performance

---

_Issue opened by @Avasam on 2023-10-03 03:08_

https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb140-use-starmap mentions performance.

https://docs.astral.sh/ruff/rules/reimplemented-starmap/ does not

My local tests seem to agree:
```py
from itertools import starmap
from timeit import timeit

def foo(a: int, b: str): pass

big_list = ["*"] * 99

def test_generator_comprehension():
    return (
        foo(index, value) for index, value
        in enumerate(big_list)
    )

def test_list_comprehension():
    return [
        foo(index, value) for index, value
        in enumerate(big_list)
    ]

def test_generator_starmap():
    return starmap(
        foo,
        enumerate(big_list),
    )

def test_list_starmap():
    return list(
        starmap(
            foo,
            enumerate(big_list),
        ),
    )

print("test_generator_comprehension", timeit(test_generator_comprehension))
print("test_list_comprehension", timeit(test_list_comprehension))
print("test_generator_starmap", timeit(test_generator_starmap))
print("test_list_starmap", timeit(test_list_starmap))

```

Python 3.9.13:
```
test_generator_comprehension 0.3537939
test_list_comprehension 9.4948082
test_generator_starmap 0.22116740000000057
test_list_starmap 6.5608599000000005
```

Python 3.11.2 (surprisingly the generator comprehension is even slower in Python 3.11, despite the proclaimed comprehension speed improvements, list comprehension checks out):
```
test_generator_comprehension 0.46688559994800016
test_list_comprehension 8.789011800021399
test_generator_starmap 0.24132720002671704
test_list_starmap 7.120547099970281
```

Scales similarly with a list of ~100 or ~1million items (the list examples takes forever though)

I think it would be important to mention as well. It is not insignificant (a little bit for list, ~2x for generators) and the doc makes it sounds like a simple stylistic change.

---

_Label `documentation` added by @charliermarsh on 2023-10-03 03:13_

---

_Label `good first issue` added by @zanieb on 2023-10-03 03:16_

---

_Comment by @JelleZijlstra on 2023-10-04 02:03_

On Python 3.12 the listcomp is faster (likely thanks to PEP 709). Running @Avasam's benchmark:

```
test_generator_comprehension 0.19647120800800622
test_list_comprehension 3.4521896250080317
test_generator_starmap 0.15749416704056785
test_list_starmap 3.6789443339803256
```

(The numbers for generators don't feel very meaningful as it's just for creating the generator, not iterating over it.)

---

_Comment by @tjkuson on 2023-10-07 09:54_

I ran the code passing the generators to `all` to make them actually do things.

On Python 3.11:

```
test_generator_comprehension 3.9400883330017678
test_generator_starmap 2.439227167000354
```

On Python 3.12:

```
test_generator_comprehension 4.032705583002098
test_generator_starmap 3.4624144999979762
```

So it looks like `itertools.starmap` is faster for generators, just not comprehensions.

(Accidentally deleted previous comment instead of editing it when trying to correct numbers, apologises for the noise.)

---

_Closed by @charliermarsh on 2023-10-07 13:27_

---
