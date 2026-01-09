---
number: 10463
title: "Rule Suggestion `min`/`max` instead of `sorted(l)[0]`"
type: issue
state: closed
author: Skylion007
labels:
  - rule
assignees: []
created_at: 2024-03-18T21:17:00Z
updated_at: 2024-04-23T01:13:59Z
url: https://github.com/astral-sh/ruff/issues/10463
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule Suggestion `min`/`max` instead of `sorted(l)[0]`

---

_Issue opened by @Skylion007 on 2024-03-18 21:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Saw this in code:
```python
a = sorted(l)[0]
a = sorted(l, reverse=True)[-1] # not sure about this one....
```
should be
```python
a = min(l)
```

Additionally, the following code pattern can also be simplified

```python
a = sorted(l, reverse=True)[0] 
a = sorted(l)[-1] # not sure about this, sort stability could make it output a different value, right?
```
should be
```python
a = max(l)
```

Is there already a pylint rule for this? I wouldn't be surprised. If not, let's add it as a RUFF rule.

I am not sure about the `[-1]` variants, as there may be some subtlety there with sort stability?

---

_Label `rule` added by @charliermarsh on 2024-03-18 21:19_

---

_Comment by @ottaviohartman on 2024-03-18 23:44_

This seems nice for my first rule contribution. It occurs way more than I was expecting from [this GitHub search](https://github.com/search?q=%2Fsorted%5C%28.*%5C%29%5C%5B0%5C%5D%2F+language%3APython&type=code&p=2), though some of these are not actual cases of this violation.

`min()` and `max()` support a `key` argument as well, so the fix would forward that to these functions.

> Is there already a pylint rule for this? I wouldn't be surprised. If not, let's add it as a RUFF rule.

Not from a quick search in their docs.

@charliermarsh do you think this is something I could tackle? Any guidance would be nice but not necessary until I hit roadblocks.

## Performance impact of fixing this violation

Test script
```python
import timeit
import random

def method_sorted(a):
    return sorted(a)[0]

def method_min(a):
    return min(a)

sizes = [10**i for i in range(1, 5)]  # Sizes: 10, 100, 1000, ...
number_of_executions = 1000

for size in sizes:
    a = [random.randint(1, 1000) for _ in range(size)]
    
    time_sorted = timeit.timeit(lambda: method_sorted(a), number=number_of_executions)
    time_min = timeit.timeit(lambda: method_min(a), number=number_of_executions)
    
    speedup = time_sorted / time_min
    print(f"Size: {size}, sorted(a)[0] time: {time_sorted:.5f}s, min(a) time: {time_min:.5f}s, Speedup: {speedup:.2f}x")
```

On my M1 pro:
```python
Size: 10, sorted(a)[0] time: 0.00024s, min(a) time: 0.00021s, Speedup: 1.17x
Size: 100, sorted(a)[0] time: 0.00279s, min(a) time: 0.00118s, Speedup: 2.36x
Size: 1000, sorted(a)[0] time: 0.05002s, min(a) time: 0.01093s, Speedup: 4.58x
Size: 10000, sorted(a)[0] time: 0.88557s, min(a) time: 0.10932s, Speedup: 8.10x
```

Results are less impressive, but still good, with `key=lambda x: x`:
```python
Size: 10, sorted(a)[0] time: 0.00067s, min(a) time: 0.00065s, Speedup: 1.03x
Size: 100, sorted(a)[0] time: 0.00629s, min(a) time: 0.00433s, Speedup: 1.45x
Size: 1000, sorted(a)[0] time: 0.08372s, min(a) time: 0.04047s, Speedup: 2.07x
Size: 10000, sorted(a)[0] time: 1.23920s, min(a) time: 0.40743s, Speedup: 3.04x
```

---

_Referenced in [dosisod/refurb#332](../../dosisod/refurb/issues/332.md) on 2024-03-19 14:02_

---

_Renamed from "Rule Suggestion min/max instead of `sorted(l)[0]`" to "Rule Suggestion `min`/`max` instead of `sorted(l)[0]`" by @Skylion007 on 2024-03-19 14:03_

---

_Comment by @dosisod on 2024-03-20 06:54_

Just implemented this as FURB192 in Refurb: https://github.com/dosisod/refurb/pull/333

---

_Comment by @hikaru-kajita on 2024-03-20 07:42_

Python `sort` and `sorted` are [guaranteed to be stable](https://docs.python.org/3/howto/sorting.html#sort-stability-and-complex-sorts), so it could have problem like this:
```python
from operator import itemgetter
data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]

min(data, key=itemgetter(0))  # ('blue', 1)
sorted(data, key=itemgetter(0))[0]  # ('blue', 1)
sorted(data, key=itemgetter(0), reverse=True)[-1]  # ('blue, 2')
```
In this case, the order of `('red', 1), ('red', 2)` and `('blue', 1), ('blue', 2)` is retained for both `sorted` call, which cause the result of the two to become different.
So we need to be careful when adding the rule for `[-1]`, though I think it is not very frequent to intentionally use `sorted` function that way.

---

_Referenced in [dosisod/refurb#333](../../dosisod/refurb/pulls/333.md) on 2024-03-20 13:55_

---

_Comment by @zanieb on 2024-03-20 14:23_

I would be okay documenting that as a caveat of the rule, seems like a weird edge-case.

---

_Comment by @Skylion007 on 2024-03-22 15:29_

We should probably flag those unstable sorts as unsafe fixes if we go through with them.

---

_Comment by @zanieb on 2024-03-22 16:14_

How can we identify them as unstable?

---

_Comment by @Skylion007 on 2024-03-30 15:28_

We should test it out using the example provided above but I think only the '[-1]' index pattern is unstable.

---

_Comment by @Skylion007 on 2024-03-30 15:32_

Actually to clarify, all these sorts are stable, but reverse=True reverse the order unstable elements apparently. (And reverse=True, '[-1]'). So only the first proposed fix using min (and it's reversed version) will always be guaranteed to be the same. I suppose. Maybe that warrants its own rule in that case or a rule toggle not flag it when it could change the returned value on ties.

---

_Comment by @ottaviohartman on 2024-04-02 00:27_

I'd like to start working on this if nobody has started yet :)

---

_Assigned to @ottaviohartman by @MichaReiser on 2024-04-02 09:13_

---

_Referenced in [astral-sh/ruff#10868](../../astral-sh/ruff/pulls/10868.md) on 2024-04-11 00:17_

---

_Closed by @charliermarsh on 2024-04-23 01:13_

---
