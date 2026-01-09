---
number: 1640
title: Suggest type annotations similar to autotyping
type: issue
state: closed
author: twoertwein
labels:
  - fixes
  - plugin
assignees: []
created_at: 2023-01-04T22:12:50Z
updated_at: 2023-11-14T04:34:16Z
url: https://github.com/astral-sh/ruff/issues/1640
synced_at: 2026-01-07T13:12:14-06:00
---

# Suggest type annotations similar to autotyping

---

_Issue opened by @twoertwein on 2023-01-04 22:12_

[autotyping](https://github.com/JelleZijlstra/autotyping) suggests type annotations in a "safe" and more "aggressive" manner. It would be nice to include its rules in ruff.

---

_Label `plugin` added by @charliermarsh on 2023-01-05 04:57_

---

_Comment by @charliermarsh on 2023-01-05 04:57_

That's a neat library. We could definitely support this.

---

_Comment by @charliermarsh on 2023-01-05 05:04_

This would benefit from #835.

---

_Comment by @charliermarsh on 2023-01-05 05:15_

I think this should probably be framed as autofix for the flake8-annotation rules, with settings to enable or disable the various heuristics. That way, we avoid introducing multiple rules around annotation presence.

---

_Label `autofix` added by @charliermarsh on 2023-01-05 05:15_

---

_Label `good first issue` added by @charliermarsh on 2023-01-05 05:15_

---

_Comment by @sbrugman on 2023-02-16 08:20_

Since the `autotyping` package contains a guessing strategy, it would benefit from autofix aggressiveness levels (https://github.com/charliermarsh/ruff/issues/1997)

---

_Comment by @sbrugman on 2023-02-16 15:10_

Ok tried this out and it's simple but pretty neat simple start: when default arguments are present it's pretty easy to infer the scalar types. Similarly, if no return statement is present the `None` type is easy too.

---

_Label `good first issue` removed by @charliermarsh on 2023-02-17 23:45_

---

_Comment by @sbrugman on 2023-02-18 16:54_

For anyone interested in picking this up: this potentially is one of the most impactful autofixes to offer. 

In 69 major open-source projects annotations account for 3 of the top-5 violations:

- ANN001  226.197   Missing type annotation for function argument `unique`
- ANN101   202.229   Missing type annotation for `self` in method
- ANN201  143.044   Missing return type annotation for public function `setup`

Even fixing a subset of these 500.000 annotations will hugely reduce manual efforts needed to move to typed libraries.

The safest autofixes are:
- None return
- Scalar return
- Annotate magics



---

_Referenced in [astral-sh/ruff#3633](../../astral-sh/ruff/pulls/3633.md) on 2023-03-20 18:35_

---

_Comment by @twoertwein on 2023-07-27 13:43_

Are there plans to add the (potentially controversial) parameter annotations from autotyping as autofixes for ANN001:
`def foo(x=True)` -> `def foo(x:bool=True)`
`def foo(x=1)` -> `def foo(x:int=1)`
`def foo(x=1.0)` -> `def foo(x:float=1.0)`
`def foo(x="")` -> `def foo(x:str="")`
`def foo(x=b"")` -> `def foo(x:bytes=b"")`

Pyright uses non-none default values to infer missing annotations, mypy doesn't. I believe people who enable ANN, would welcome this autofix as they are anyway working on adding type annotations (low "cost/frustration" of false positives).

---

_Comment by @tylerlaprade on 2023-07-27 14:38_

@twoertwein, as a Pyright user, I would prefer the option to exclude these cases from `ANN001`, as suggested in #5958. I find the unnecessary annotations can make it harder to visually parse a function.

---

_Comment by @twoertwein on 2023-07-27 15:04_

> @twoertwein, as a Pyright user, I would prefer the option to exclude these cases from ANN001, as suggested in https://github.com/astral-sh/ruff/issues/5958. I find the unnecessary annotations can make it harder to visually parse a function.

If it was a PEP that would mandate that missing type annotations should be the type of a non-None default value (or if all major type checkers would do it anyway), I would be inclined to agree with you (I still prefer explicit over implicit and it would look more consistent). When I see an un-annotated parameter, my thought is not it must be the default value's type but it was probably too difficult to annotate it (a more complex union of types).

Pyright is my type checker of choice :) but most open source projects use mypy or use multiple type checkers (for example pandas started with mypy and is slowly inching to enable more pyright rules). Adding those annotations would help that different type checkers behave more similar. It would also narrow the gap between autotyping and ruff (surprise: ruff is way faster) :)

---

_Comment by @tylerlaprade on 2023-07-27 15:47_

Sure, I'm not against the option existing for those who want it! Apologies if I came across that way. I want it to exist, I just don't want it forced upon Pyright and Basedmypy users.

---

_Referenced in [pandas-dev/pandas#54260](../../pandas-dev/pandas/pulls/54260.md) on 2023-07-28 13:52_

---

_Referenced in [astral-sh/ruff#8643](../../astral-sh/ruff/pulls/8643.md) on 2023-11-13 04:44_

---

_Closed by @charliermarsh on 2023-11-14 04:34_

---
