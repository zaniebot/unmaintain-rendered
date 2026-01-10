---
number: 2443
title: "[ruff --fix] (v0.0.239) When working with numpy arrays `mask == True` is not the same as `mask is True`"
type: issue
state: closed
author: FrancescElies
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-02-01T15:41:19Z
updated_at: 2023-05-21T16:06:02Z
url: https://github.com/astral-sh/ruff/issues/2443
synced_at: 2026-01-10T01:22:40Z
---

# [ruff --fix] (v0.0.239) When working with numpy arrays `mask == True` is not the same as `mask is True`

---

_Issue opened by @FrancescElies on 2023-02-01 15:41_

After running `ruff --fix` the last line of the following script gets reformatted as `np.where(mask is True)` but in this context is not the same.

```python
import numpy as np

a = np.arange(10)
mask = a > 5
np.where(mask == True)  # equivalent to `mask.nonzero()
```

Maybe in that context one should not be using `np.where(mask == True)`  but it's equivalent `mask.nonzero()` instead, at anyrate we have some old code using that pattern.

See below an interactive session showing is not the same.
```python
In [2]: import numpy as np
In [13]: a = np.arange(10)

In [14]: mask = a > 5

In [15]: np.where(mask == True)
Out[15]: (array([6, 7, 8, 9], dtype=int64),)

In [18]: np.where(mask is True)
Out[18]: (array([], dtype=int64),)
```

What do you think about this corner case?

---

_Label `bug` added by @charliermarsh on 2023-02-01 17:39_

---

_Comment by @charliermarsh on 2023-02-01 17:39_

Ah yeah, this has come up before. Unfortunately it's really hard to avoid without disabling the rule entirely, though I'm open to suggestions on heuristics.

My current suggestion has been to disable that rule if you're working in any context that requires boolean comparisons like that.


---

_Comment by @spaceone on 2023-02-01 18:37_

This could potentially also happen with sqlalchemy I guess.
There you have `query.where(foo==bar)` queries.

---

_Comment by @FrancescElies on 2023-02-02 06:41_

> Ah yeah, this has come up before. Unfortunately it's really hard to avoid without disabling the rule entirely, though I'm open to suggestions on heuristics.

Can you point me out in the direction where this code lives?
Which kind of info do we have in that context, what could we use? regex? do we have maybe the variable type available (like pyright or mypy does)?

It looks like `{something}.where({avar} == {True|False})` might help but for sure there will be other cases too, thus ... not sure what to suggest.

> My current suggestion has been to disable that rule if you're working in any context that requires boolean comparisons like that.

On the other hand at least in our case we have many people coming from the c-embedded world and writing python in a c-ish way and in general I find it also helpful.

---

_Comment by @conbrad on 2023-02-16 00:20_

> This could potentially also happen with sqlalchemy I guess. There you have `query.where(foo==bar)` queries.

Verified today this is the case.

---

_Comment by @conbrad on 2023-02-16 00:28_

Potentially something that could be defined as a `Trivia`: https://github.com/charliermarsh/ruff/blob/main/crates/ruff_python_formatter/src/trivia.rs#L46

E.g. a `FilterFunction`. 

Not sure if it would make more sense to do the analysis at the AST level though, and check for `sqlalchemy` or `numpy` parent nodes.

---

_Referenced in [bcgov/wps#2650](../../bcgov/wps/pulls/2650.md) on 2023-02-16 17:51_

---

_Comment by @charliermarsh on 2023-03-14 17:58_

We could consider ignoring these expressions inside of all `.where` calls, regardless of whether it's NumPy, SQLAlchemy, etc. We'd be trading off false negatives for false positives.

---

_Comment by @conbrad on 2023-03-14 20:13_

> We could consider ignoring these expressions inside of all `.where` calls, regardless of whether it's NumPy, SQLAlchemy, etc. We'd be trading off false negatives for false positives.

I think this would wind up chasing a lot of stragglers. E.g. `.filter`, `.having` are just a couple of other predicate functions in SQLAlchemy that expect `==` over `is` I believe.

---

_Comment by @JonathanPlasse on 2023-03-14 20:48_

Maybe this should wait until we can determine the type of objects.

---

_Comment by @zanieb on 2023-04-24 17:27_

To share another real world example, I encountered this using ruff in a project that uses SQLAlchemy. Ruff fixed comparisons with `None` to use `is`/`is_not` instead of `==`/`!=` but this broke our SQLAlchemy filters. As a solution, I've switched to use of explicit `.is_` and `.is_not` SQLAlchemy operators when doing comparisons with `None` which seems like better practice anyway.

https://github.com/PrefectHQ/prefect/pull/9283/commits/1b645872a14b9b5a3e50ef39b03adcc81436adb4

Our usage seems harder to detect as a special case since we construct the list of filters _outside_ of the call. It only seems feasible to avoid by detecting the type of the object in the comparison.

---

_Referenced in [PrefectHQ/prefect#9283](../../PrefectHQ/prefect/pulls/9283.md) on 2023-04-24 17:39_

---

_Comment by @conbrad on 2023-04-24 18:44_

> To share another real world example, I encountered this using ruff in a project that uses SQLAlchemy. Ruff fixed comparisons with `None` to use `is`/`is_not` instead of `==`/`!=` but this broke our SQLAlchemy filters. As a solution, I've switched to use of explicit `.is_` and `.is_not` SQLAlchemy operators when doing comparisons with `None` which seems like better practice anyway.

Wonder if those operators were influenced by the recent language/tooling co-design. I seem to remember having issues with the `is` operators not behaving the same as the `==` ones but that may have just been a PEBKAC issue.

---

_Comment by @zanieb on 2023-04-25 01:11_

https://github.com/charliermarsh/ruff/issues/1852 has some previous discussion as well.

@charliermarsh if you'd like I can open a new issue that tracks the general problem replacing `==` with `is` and enumerate the datatypes this is an issue for so you can close out duplicates that are for specific cases?

---

_Referenced in [astral-sh/ruff#4356](../../astral-sh/ruff/issues/4356.md) on 2023-05-10 18:40_

---

_Comment by @JonathanPlasse on 2023-05-21 09:11_

Could be labeled `type-inference`

---

_Label `type-inference` added by @charliermarsh on 2023-05-21 13:28_

---

_Referenced in [astral-sh/ruff#4560](../../astral-sh/ruff/issues/4560.md) on 2023-05-21 15:51_

---

_Comment by @zanieb on 2023-05-21 16:01_

Here's the aforementioned tracking issue https://github.com/charliermarsh/ruff/issues/4560

---

_Comment by @charliermarsh on 2023-05-21 16:06_

Thanks tons @madkinsz 

---

_Closed by @charliermarsh on 2023-05-21 16:06_

---
