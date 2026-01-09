---
number: 10137
title: "Rule request: Require uppercase variables to be typed with `typing.Final`"
type: issue
state: open
author: adamtheturtle
labels:
  - rule
assignees: []
created_at: 2024-02-26T14:30:17Z
updated_at: 2024-02-28T07:18:56Z
url: https://github.com/astral-sh/ruff/issues/10137
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule request: Require uppercase variables to be typed with `typing.Final`

---

_Issue opened by @adamtheturtle on 2024-02-26 14:30_

In my projects, when I spot an uppercase variable, I make sure to type it with `typing.Final`.

It would be nice to have this as a ruff rule.

For example, from the [Python documentation's example of `typing.Final`](https://docs.python.org/3/library/typing.html#typing.Final):

```python
MAX_SIZE: Final = 9000
MAX_SIZE += 1  # Error reported by type checker
```

I'd like:

```python
MAX_SIZE = 9000 # ruff shows an error because this is not typed with `typing.Final`.
```

---

_Comment by @AlexWaygood on 2024-02-27 10:43_

I actually suggested something like this a while back for flake8-pyi: https://github.com/PyCQA/flake8-pyi/issues/122. But we rejected the idea because it would likely cause too many false positives for stub files, for which the most important thing is the need to exactly follow what the runtime does.

For runtime Python code, there would likely be a lower number of false positives than in `.pyi` files. I think it would be a useful lint.

Not sure where it would belong -- it does feel like it fits in pretty well with flake8-pyi, but it would be the first rule in that ruleset that would run on `.py` files and not `.pyi` files 😄

---

_Label `rule` added by @AlexWaygood on 2024-02-27 10:43_

---

_Comment by @mikeleppane on 2024-02-28 07:18_

I agree. This would be a helpful rule and something that you want when following Python conventions/PEP8, assuming you are using typing. If you are not using types, this is just noise. However, I still believe this feeds the purpose.  

---
