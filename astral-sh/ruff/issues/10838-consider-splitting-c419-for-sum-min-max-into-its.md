```yaml
number: 10838
title: Consider splitting C419 for sum/min/max into its own rule
type: issue
state: open
author: carljm
labels:
  - breaking
  - needs-decision
  - linter
assignees: []
created_at: 2024-04-08T19:26:37Z
updated_at: 2025-10-06T17:37:47Z
url: https://github.com/astral-sh/ruff/issues/10838
synced_at: 2026-01-10T11:09:53Z
```

# Consider splitting C419 for sum/min/max into its own rule

---

_Issue opened by @carljm on 2024-04-08 19:26_

As discussed in https://github.com/astral-sh/ruff/issues/3259#issuecomment-2033127339, the performance impact of switching from comprehension to generator is different if you are passing to a short-circuiting function (e.g. `any` or `all`) vs a non-short-circuiting one (`sum`, `min`, `max`). Given these differing tradeoffs, it would be reasonable for a project to want to enable the lint for short-circuiting cases, and not the others. This suggests that we should split them into separate rules (or add a config option), as requested in https://github.com/astral-sh/ruff/pull/10759#issuecomment-2037394814


---

_Label `needs-decision` added by @carljm on 2024-04-08 19:27_

---

_Label `linter` added by @carljm on 2024-04-08 19:27_

---

_Added to milestone `v0.4.0` by @carljm on 2024-04-08 19:33_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 09:11_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-24 09:12_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-24 09:12_

---

_Comment by @hauntsaninja on 2024-06-27 19:19_

I would also like these to be separate rules, because of the performance impact!

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-12 08:50_

---

_Comment by @Andrej730 on 2025-06-15 05:48_

Also came across this trying out rule updates from `--preview`. 
Generator is catching up but only when there are a lot of objects to hold in memory (>100k), otherwise list comprehensions are significantly faster.

```python
import timeit

def bench(repeats):
    def decorator(func):
        total = timeit.timeit(func, number=repeats)
        avg = total / repeats
        print(f"{func.__name__: <35} - {avg * 1e6:.2f} μs")
        return func
    return decorator

@bench(repeats=10000)
def test_sum_list_comprehension_4000():
    sum([i for i in range(1, 4000)])

@bench(repeats=10000)
def test_sum_generator_4000():
    sum(i for i in range(1, 4000))

@bench(repeats=100)
def test_sum_list_comprehension_100000():
    sum([i for i in range(1, 100000)])

@bench(repeats=100)
def test_sum_generator_100000():
    sum(i for i in range(1, 100000))

# test_sum_list_comprehension_4000    - 63.88 μs
# test_sum_generator_4000             - 107.81 μs
# test_sum_list_comprehension_100000  - 2700.70 μs
# test_sum_generator_100000           - 3106.45 μs
```

---

_Comment by @jvacek on 2025-10-06 17:37_

Performance aside, I think it's is worth reiterating that the preview behaviour is deviating from how C419 behaves like upstream, as it's pretty explicit about only affecting `any` and `all`.

https://github.com/adamchainz/flake8-comprehensions?tab=readme-ov-file#c419-unnecessary-list-comprehension-in-anyall-prevents-short-circuiting---rewrite-as-a-generator

---
