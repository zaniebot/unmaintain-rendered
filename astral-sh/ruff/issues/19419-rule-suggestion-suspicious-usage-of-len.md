```yaml
number: 19419
title: "Rule suggestion: suspicious usage of `len()`"
type: issue
state: open
author: kaddkaka
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-18T09:37:18Z
updated_at: 2025-07-28T21:51:08Z
url: https://github.com/astral-sh/ruff/issues/19419
synced_at: 2026-01-12T15:54:56Z
```

# Rule suggestion: suspicious usage of `len()`

---

_@kaddkaka_

### Summary

Calls to len that will have a constant return value are likely a mistake, see these examples:

```py
len(())
len([])
len({})
len([[x for x in range(5)]])  # accidental 2 pairs of brackets, see real life example below [1]
```

1. https://github.com/chipsalliance/riscv-dv/blob/7e54b678ab7499040336255550cdbd99ae887431/pygen/pygen_src/isa/riscv_cov_instr.py#L365


Building a list to count, could instead recommend `sum()`:

```py
len([1 for  item in big_container if item.is_common()])

# replace with below to avoid creating a list
sum(1 for  item in big_container if item.is_common())
```

---

_Renamed from "Strange usage of `len()`" to "Rule suggestion: suspicious usage of `len()`" by @kaddkaka on 2025-07-18 09:38_

---

_Label `rule` added by @MichaReiser on 2025-07-18 13:20_

---

_Label `needs-decision` added by @MichaReiser on 2025-07-18 13:20_

---

_Comment by @Avasam on 2025-07-23 04:04_

> Building a list to count, could instead recommend `sum()`:
> ```py
> len([1 for  item in big_container if item.is_common()])
> 
> # replace with below to avoid creating a list
> sum(1 for  item in big_container if item.is_common())
> ```

So I initially thought the same, but I wrote a small benchmark:
```py
from timeit import timeit

big_list = list(range(10_000))

print(
    "len          ",
    timeit(lambda: len([e for e in big_list if e % 2]), number=10_000),
)
print(
    "len bool     ",
    timeit(lambda: len([True for e in big_list if e % 2]), number=10_000),
)
print(
    "sum list     ",
    timeit(lambda: sum([1 for e in big_list if e % 2]), number=10_000),
)
print(
    "sum generator",
    timeit(lambda: sum(1 for e in big_list if e % 2), number=10_000),
)
```

Results: 
`uvx --python=3.9 python test.py`
```
len           1.7165404000000002
len bool      1.6241272000000002
sum list      1.8865444000000005
sum generator 2.4472100999999995
```

`uvx --python=3.13 python test.py`
```
len           2.3589852999975847
len bool      2.3424118000002636
sum list      2.4642254000027606
sum generator 2.8652036000021326
```

---

_Comment by @kaddkaka on 2025-07-28 13:36_

> So I initially thought the same, but I wrote a small benchmark:

Thanks for the benchmark, that is interesting 🤔 

---

_Comment by @kaddkaka on 2025-07-28 21:51_

Apparently it's related to that a generator expression will repeatedly start and pause a code object.

Similar generator expression together with any/all/tuple has been optimized with this PR to cpython: https://github.com/python/cpython/issues/131738

ruff has a discussion related to performance around similar code: https://github.com/astral-sh/ruff/issues/16904

I'm not sure what this entails exactly..

---
