```yaml
number: 12648
title: flake8-comprehensions (C4) - Remove unnecessary list comprehensions in additional contexts that can take arbitrary iterables (C419)
type: issue
state: closed
author: gandhis1
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2024-08-02T22:01:13Z
updated_at: 2024-08-05T12:10:32Z
url: https://github.com/astral-sh/ruff/issues/12648
synced_at: 2026-01-12T15:54:52Z
```

# flake8-comprehensions (C4) - Remove unnecessary list comprehensions in additional contexts that can take arbitrary iterables (C419)

---

_@gandhis1_

The following code all creates unnecessary list comprehensions

```python
    a = all([i is not None for i in range(10)])
    b = tuple([i * 2 for i in range(10)])
    c = sum([i * 2 for i in range(10)])
    d = ",".join([str(i * 2) for i in range(10)])
```

ruff will only suggest changes on the first line:

```
stuff.py:3:13: C419 Unnecessary list comprehension
  |
2 | def main() -> None:
3 |     a = all([i is not None for i in range(10)])
  |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ C419
4 |     b = tuple([i * 2 for i in range(10)])
5 |     c = sum([i * 2 for i in range(10)])
  |
  = help: Remove unnecessary list comprehension
```

However all of these are unnecessary list comprehensions. It ought to be able to identify them all. For one, the [docs suggest](https://docs.astral.sh/ruff/rules/unnecessary-comprehension-in-call/) that `sum` is covered, but from the above, it appears to not fully be.




---

_Comment by @charliermarsh on 2024-08-02 22:11_

`sum` is covered under `--preview`. 

---

_Comment by @charliermarsh on 2024-08-02 22:12_

Honestly not sure why `tuple` doesn't have any corresponding rules.

---

_Comment by @charliermarsh on 2024-08-02 22:13_

Oh, I think it's because `tuple([1, 2])` gets rewritten to `(1, 2)` (so the rule only supports literals). We could extend it to remove the inner list.

---

_Label `rule` added by @charliermarsh on 2024-08-02 22:13_

---

_Comment by @charliermarsh on 2024-08-02 23:25_

I think we should expand C409 to include comprehensions.

---

_Label `good first issue` added by @charliermarsh on 2024-08-02 23:25_

---

_Label `help wanted` added by @charliermarsh on 2024-08-02 23:25_

---

_Comment by @charliermarsh on 2024-08-02 23:26_

`C419` is an example of how that logic would work -- it's nearly identical. (This would be behind `--preview` for now.)

---

_Comment by @eth3lbert on 2024-08-03 01:43_

While `tuple` and `"".join` can be rewritten without list comprehensions, it doesn't necessarily mean there's a performance improvement.

```ipython
In [1]: %timeit tuple([i for i in range(1000)])
20.4 μs ± 148 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

In [2]: %timeit tuple(i for i in range(1000))
39.7 μs ± 205 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

In [3]: %timeit ",".join([str(i * 2) for i in range(10)])
1.02 μs ± 22.6 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)

In [4]: %timeit ",".join(str(i * 2) for i in range(10))
1.49 μs ± 14.4 ns per loop (mean ± std. dev. of 7 runs, 1,000,000 loops each)
```

---

_Comment by @charliermarsh on 2024-08-03 12:22_

I believe the same is true for `sum` et al which is why they're still in preview and we considered adding a separate rule for them.

---

_Comment by @dylwil3 on 2024-08-04 00:35_

It seems like a toss up to me whether it should be `C409` or `C419` that should be expanded to include this violation/fix. In the PR above I did what was suggested above (and extended `C409`) - but let me know if you have second thoughts and I can extend `C419` instead.

---

_Closed by @charliermarsh on 2024-08-05 02:14_

---

_Closed by @charliermarsh on 2024-08-05 02:14_

---

_Comment by @gandhis1 on 2024-08-05 12:10_

What about `str.join`?

---
