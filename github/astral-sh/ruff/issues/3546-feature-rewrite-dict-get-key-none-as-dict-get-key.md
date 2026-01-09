---
number: 3546
title: "Feature: rewrite dict.get(key, None) as dict.get(key)"
type: issue
state: closed
author: janosh
labels:
  - rule
assignees: []
created_at: 2023-03-15T18:26:20Z
updated_at: 2023-04-20T13:53:26Z
url: https://github.com/astral-sh/ruff/issues/3546
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature: rewrite dict.get(key, None) as dict.get(key)

---

_Issue opened by @janosh on 2023-03-15 18:26_

This is not in [`flake8-simplify`](https://github.com/MartinThoma/flake8-simplify) AFAIK but would prob best fit in that rule class.

From the flake8-simplify readme:

> Simplifying usage of dictionaries:
> 
> https://github.com/MartinThoma/flake8-simplify/issues/72: Use 'a_dict.get(key, "default_value")' instead of an if-block ([example](https://github.com/MartinThoma/flake8-simplify#SIM401))
> https://github.com/MartinThoma/flake8-simplify/issues/40: Use 'key in dict' instead of 'key in dict.keys()' ([example](https://github.com/MartinThoma/flake8-simplify#SIM118))


---

_Label `rule` added by @charliermarsh on 2023-03-15 23:23_

---

_Comment by @charliermarsh on 2023-03-15 23:23_

Yeah just not sure what code this would fall under...

---

_Referenced in [MartinThoma/flake8-simplify#171](../../MartinThoma/flake8-simplify/issues/171.md) on 2023-03-15 23:32_

---

_Comment by @janosh on 2023-03-28 22:52_

Rule name in `flake8-simplify` [will be SIM910](https://github.com/MartinThoma/flake8-simplify/issues/171#issuecomment-1487693798).

---

_Comment by @charliermarsh on 2023-03-28 23:00_

Oh nice!

---

_Comment by @kyoto7250 on 2023-03-29 01:44_

I am currently working for making this rule in `flake8-simplify`, and I would also like to implement the rule in `ruff`.

---

_Referenced in [astral-sh/ruff#3874](../../astral-sh/ruff/pulls/3874.md) on 2023-04-04 03:00_

---

_Closed by @charliermarsh on 2023-04-04 03:52_

---

_Comment by @Borda on 2023-04-20 10:09_

> I would also like to implement the rule in `ruff`

cool, was this already added? I see in the docs that it shall be implemented, but with 0.0.262 these cases are not flagged or repaired... could be some typing limitation that is not properly recognized? But `flake8-simplify` detects them...

https://github.com/Lightning-AI/lightning/blob/8dac2512733bae034f437334a71e4c114d458a7c/src/lightning/pytorch/profilers/pytorch.py#L322
```
src/lightning/pytorch/profilers/pytorch.py:322:20: SIM910 Use 'profiler_kwargs.get("schedule")' instead of 'profiler_kwargs.get('schedule', None)'
```

https://github.com/Lightning-AI/lightning/blob/8dac2512733bae034f437334a71e4c114d458a7c/src/lightning/pytorch/utilities/parsing.py#L102
```
src/lightning/pytorch/utilities/parsing.py:102:16: SIM910 Use 'local_vars.get(self_var)' instead of 'local_vars.get(self_var, None)'
```

---

_Referenced in [Lightning-AI/pytorch-lightning#17386](../../Lightning-AI/pytorch-lightning/pulls/17386.md) on 2023-04-20 11:06_

---

_Comment by @dhruvmanila on 2023-04-20 11:11_

This is implemented and released. In `ruff`, this will only work on dict literals (`{}`) while in `flake8-simplify` this works with any call which resembles the expression (`<name>.get("...", None)`). I'm assuming this was done as otherwise it would be difficult to determine if the variable is actually a dictionary and not any other object. This would be solved in the future when type-inference is implemented.

\cc @charliermarsh I think we should make it explicit in the documentation about this difference.

---

_Comment by @janosh on 2023-04-20 13:27_

> This is implemented and released. In ruff, this will only work on dict literals ({}) while in flake8-simplify this works with any call which resembles the expression (<name>.get("...", None))

Ah, I was wondering if that was the case. I think mentioning in the docs would be great.

---

_Comment by @kyoto7250 on 2023-04-20 13:53_

I implemented this rule, but this difference in behavior was not intended.

@charliermarsh 

I want to fix this behavior if possible.
Is there any way to find out the type of a variable when checking ruff?


---
