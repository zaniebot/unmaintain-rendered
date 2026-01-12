```yaml
number: 9592
title: "Rule suggestion: recommend `dict.fromkeys` over comprehension"
type: issue
state: closed
author: tjkuson
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-01-20T14:00:42Z
updated_at: 2024-01-24T02:16:49Z
url: https://github.com/astral-sh/ruff/issues/9592
synced_at: 2026-01-12T15:54:49Z
```

# Rule suggestion: recommend `dict.fromkeys` over comprehension

---

_@tjkuson_

Replace

```python
{elt: None for elt in foo}
```

with

```python
dict.fromkeys(foo)
```

`dict.fromkeys` is more efficient and readable.

```
In [4]: import random

In [5]: choices = ("red", "green", "blue")

In [6]: dupes = [random.choice(choices) for _ in range(1000)]

In [7]: %timeit list({elt: None for elt in dupes})
18.8 µs ± 97.8 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)

In [8]: %timeit list(dict.fromkeys(dupes))
12.6 µs ± 49.9 ns per loop (mean ± std. dev. of 7 runs, 100,000 loops each)
```

Efficiency boost tested on both Python 3.9 and 3.12.

Note: would have to skip this rule for mutable values (see #4613).

---

_Comment by @charliermarsh on 2024-01-20 15:35_

Looks great!

---

_Label `rule` added by @charliermarsh on 2024-01-20 15:35_

---

_Label `accepted` added by @charliermarsh on 2024-01-20 15:35_

---

_Comment by @mikeleppane on 2024-01-20 21:41_

I could pick up this one?

---

_Comment by @charliermarsh on 2024-01-20 23:00_

Feel free unless @tjkuson planned to do it or had started on it -- I'll leave it to @tjkuson to confirm!

---

_Comment by @tjkuson on 2024-01-20 23:33_

> Feel free unless @tjkuson planned to do it or had started on it -- I'll leave it to @tjkuson to confirm!

Haven't started anything, have at it! @mikeleppane 

---

_Comment by @Skylion007 on 2024-01-21 06:33_

This would be a great rule for refurb, C4, or perflint. You should consider pinging the author of those libraries as well.

---

_Comment by @johnslavik on 2024-01-21 14:56_

This applies not only to `None`, but any constant.
In my opinion, `{elt: 0 for elt in foo}` should also be replaced with `dict.fromkeys(foo, 0)`.

---

_Comment by @johnslavik on 2024-01-21 14:58_

@tjkuson @charliermarsh would you agree? ↑

---

_Comment by @tjkuson on 2024-01-21 15:00_

> @tjkuson would you agree? ↑

Yes, this was my intention with the issue. Admittedly, I communicated that poorly by using only examples with `None`. (I mostly use `dict.fromkeys` in the context of ordered deduplication.)

---

_Comment by @charliermarsh on 2024-01-24 02:16_

Closed by https://github.com/astral-sh/ruff/pull/9613. Thanks @mikeleppane!

---

_Closed by @charliermarsh on 2024-01-24 02:16_

---
