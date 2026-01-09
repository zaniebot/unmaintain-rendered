---
number: 14835
title: "`FURB140` (reimplemented-starmap) suggests `itertools.starmap` when one should actually use `map`"
type: issue
state: closed
author: tdulcet
labels:
  - documentation
  - rule
  - help wanted
assignees: []
created_at: 2024-12-08T13:25:36Z
updated_at: 2025-01-16T14:18:14Z
url: https://github.com/astral-sh/ruff/issues/14835
synced_at: 2026-01-07T13:12:16-06:00
---

# `FURB140` (reimplemented-starmap) suggests `itertools.starmap` when one should actually use `map`

---

_Issue opened by @tdulcet on 2024-12-08 13:25_

The official [Python documentation](https://docs.python.org/3/library/itertools.html#itertools.starmap) for `itertools.starmap` says:
> Used instead of [map()](https://docs.python.org/3/library/functions.html#map) when argument parameters have already been “pre-zipped” into tuples.

The example in the [Ruff documentation](https://docs.astral.sh/ruff/rules/reimplemented-starmap/) for `FURB140` suggests this fix:
```diff
-passed_all_tests = all(
-    passed_test(score, passing_score)
-    for score, passing_score in zip(scores, passing_scores)
-)
+passed_all_tests = all(starmap(passed_test, zip(scores, passing_scores)))
```
However, as you can see, the parameters were not “pre-zipped”, but instead were manually zipped by explicitly calling the `zip` function, so `starmap` should not have been used. One should actually use regular `map()` instead, which is simpler and more performant:
```diff
-passed_all_tests = all(starmap(passed_test, zip(scores, passing_scores)))
+passed_all_tests = all(map(passed_test, scores, passing_scores))
```
I would suggest changing the rule to not trigger when `zip()` is used and then using one of the examples from #7771 in the documentation instead.

In #14636, among other rules, I proposed adding new lint rule(s) to catch unnecessary comprehensions. Here are some additional examples for `map()` specifically:
```py
(func(a) for a in x)                       # ⇒ map(func, x)
(func(*a) for a in zip(x, y))              # ⇒ map(func, x, y)
(func(a, b) for a, b in zip(x, y))         # ⇒ map(func, x, y)
(func(a, b, c) for a, b, c in zip(x, y, z))# ⇒ map(func, x, y, z)
itertools.starmap(func, zip(*x))           # ⇒ map(func, *x)
itertools.starmap(func, zip(x, y))         # ⇒ map(func, x, y)
itertools.starmap(func, zip(x, y, z))      # ⇒ map(func, x, y, z)
```

CC: @dosisod ([refurb documentation](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb140-use-starmap) uses the same example)

---

_Comment by @MichaReiser on 2024-12-16 08:16_

Thanks for reporting this. Here some notes for myself:

Suggesting `starmap` when the input is already `zip`ed with `zip` isn't incorrect, it's just unnecessary because one can pass multiple iterables to `map`

```
>>> l1 = [1, 2, 3, 4]
>>> def f(a, b): print(a, b)
...
>>> import itertools
>>> list(itertools.starmap(f, zip(l1, l1)))
1 1
2 2
3 3
4 4
[None, None, None, None]
>>> list(map(f, zip(l1, l1)))
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    list(map(f, zip(l1, l1)))
    ~~~~^^^^^^^^^^^^^^^^^^^^^
TypeError: f() missing 1 required positional argument: 'b'
>>> list(map(f, l1, l1))
1 1
2 2
3 3
4 4
[None, None, None, None]
```


The next steps are:

1. Update the `FURB140` documentation to use a better example (without `zip`)
2. Exclude `map` calls where the iterable is a `zip` 
3. TBD: A new rule that recommends using `map` over `starmap(func, zip)` 
4. TBD: A new rule that detects other unnecessary usages of comprehensions where it's possible to use map instead

I suggest we start with 1 and 2, and it's probably best to discuss 3 and 4 as separate issues.

---

_Label `help wanted` added by @MichaReiser on 2024-12-16 08:16_

---

_Label `rule` added by @dhruvmanila on 2024-12-17 05:04_

---

_Referenced in [astral-sh/ruff#15481](../../astral-sh/ruff/issues/15481.md) on 2025-01-14 21:54_

---

_Label `documentation` added by @MichaReiser on 2025-01-15 13:53_

---

_Referenced in [astral-sh/ruff#15483](../../astral-sh/ruff/pulls/15483.md) on 2025-01-15 14:21_

---

_Closed by @MichaReiser on 2025-01-16 14:18_

---
