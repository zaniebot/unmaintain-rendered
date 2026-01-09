---
number: 3652
title: PLW2901 ignore when using cast
type: issue
state: closed
author: LefterisJP
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-03-21T16:57:25Z
updated_at: 2023-04-05T22:02:34Z
url: https://github.com/astral-sh/ruff/issues/3652
synced_at: 2026-01-07T13:12:14-06:00
---

# PLW2901 ignore when using cast

---

_Issue opened by @LefterisJP on 2023-03-21 16:57_

```
ruff 0.0.257
```

```python
for x, x_type in events:
     if x_type == 'something':
         x = cast(Event1, x)
     elif x_type == 'something_else':
         x = cast(Event2, x)
```

This hits with PLW2901. I guess it would make sense to ignore this error in case of `cast()` which is only used for typing.

---

_Comment by @JonathanPlasse on 2023-03-21 17:12_

In this case, this is not an error, you override `x` with a different variable `event`. This should be flagged.
However, if you cast `x` on itself like below. It makes sense to ignore `PLW2901`.
```python
for x, x_type in events:
     if x_type == 'something':
         x = cast(Event1, x)
     elif x_type == 'something_else':
         x = cast(Event2, x)
```

---

_Comment by @JonathanPlasse on 2023-03-21 17:12_

I would like to work on it.

---

_Comment by @LefterisJP on 2023-03-21 17:13_

> In this case, this is not an error, you override `x` with a different variable `event`. This should be flagged. However, if you cast `x` on itself like below. It makes sense to ignore `PLW2901`.
> 
> ```python
> for x, x_type in events:
>      if x_type == 'something':
>          x = cast(Event1, x)
>      elif x_type == 'something_else':
>          x = cast(Event2, x)
> ```

Ah sorry Jonathan, I did not run it through ruff -- tried to make a pseudocode of what I seen in our code. You are right, this is what I mean. Will edit it.

---

_Label `bug` added by @charliermarsh on 2023-03-22 03:26_

---

_Label `good first issue` added by @charliermarsh on 2023-03-29 22:29_

---

_Comment by @dhruvmanila on 2023-04-05 17:22_

> I would like to work on it.

@JonathanPlasse are you working on this?

---

_Referenced in [astral-sh/ruff#3891](../../astral-sh/ruff/pulls/3891.md) on 2023-04-05 18:53_

---

_Closed by @charliermarsh on 2023-04-05 22:02_

---
