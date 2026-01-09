---
number: 13508
title: "FURB118, `lambda x: x[:, 0]` should be `itemgetter((slice(None), 0))`, instead of `itemgetter((:, 0))`"
type: issue
state: closed
author: nasyxx
labels:
  - bug
  - rule
assignees: []
created_at: 2024-09-25T04:48:09Z
updated_at: 2024-09-26T14:20:05Z
url: https://github.com/astral-sh/ruff/issues/13508
synced_at: 2026-01-07T13:12:15-06:00
---

# FURB118, `lambda x: x[:, 0]` should be `itemgetter((slice(None), 0))`, instead of `itemgetter((:, 0))`

---

_Issue opened by @nasyxx on 2024-09-25 04:48_

In refurb, FURB118, `lambda x: x[:, 0]` should be `itemgetter((slice(None), 0))`, instead of `itemgetter((:, 0))`

```python
# xx.py
import numpy as np
xs = np.array([[1, 2, 3]])
xm = map(lambda x: x[:, 0], xs)
```

```
xx.py:
  3:10 FURB118 [*] Use `operator.itemgetter((:, 0))` instead of defining a lambda
```

I guess `(:, 0)` is not a valid tuple.

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


---

_Label `rule` added by @MichaReiser on 2024-09-25 06:58_

---

_Label `bug` added by @MichaReiser on 2024-09-25 06:58_

---

_Comment by @MichaReiser on 2024-09-25 06:58_

The python slice index syntax always trips me up and in this case it's unclear to me what the result of the map operation is. 

@nasyxx could you clarify. Do you expect the above to be valid code? I'm asking to better understand if this is a bug in the fix or that the rule shouldn't flag the above code. CC: @AlexWaygood 

---

_Comment by @nasyxx on 2024-09-25 16:03_

> @nasyxx could you clarify. Do you expect the above to be valid code?

Hi, the above is valid, while it will raise IndexError.  

My actual ndarray is quite complex, you can change `xs` to `xs = np.ones((5, 4, 3, 2, 1))`.  Then when running `list(xm)` will give you result.


---

_Comment by @AlexWaygood on 2024-09-25 17:15_

There's definitely a serious bug here: we currently suggest invalid syntax (and, if you run with `--fix`, we introduce invalid syntax).

```diff
- lambda x: x[:, 0]
+ import operator
+ operator.itemgetter((:, 0))
```

In my opinion, `itemgetter((slice(None), 0))` (which would be the correct way of rewriting this with `itemgetter`, as @nasyxx says) is quite ugly, so I think I'd vote for not emitting a diagnostic at all in this case. But others may feel differently; emitting a diagnostic would be _consistent_ with the overall (quite opinionated) philosophy of `FURB118`.

---

_Assigned to @zanieb by @zanieb on 2024-09-25 23:46_

---

_Referenced in [astral-sh/ruff#13518](../../astral-sh/ruff/pulls/13518.md) on 2024-09-26 00:02_

---

_Comment by @zanieb on 2024-09-26 01:53_

We already handle converting `x[:]` to `slice(None)` so I opted to continue emitting the diagnostic.

---

_Closed by @zanieb on 2024-09-26 14:20_

---

_Closed by @zanieb on 2024-09-26 14:20_

---
