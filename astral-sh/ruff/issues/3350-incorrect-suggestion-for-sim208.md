```yaml
number: 3350
title: Incorrect suggestion for SIM208
type: issue
state: closed
author: dosisod
labels:
  - bug
assignees: []
created_at: 2023-03-05T00:32:18Z
updated_at: 2024-08-06T17:46:38Z
url: https://github.com/astral-sh/ruff/issues/3350
synced_at: 2026-01-10T11:09:46Z
```

# Incorrect suggestion for SIM208

---

_Issue opened by @dosisod on 2023-03-05 00:32_

The `SIM208` rule should be updated to only emit errors for `not not x` in boolean contexts, since changing `not not x` to just `x` can cause different results. When used in a non-boolean context, `bool(x)` should be used instead to ensure it results in a boolean type.

## Example

```python
x = 123
y = not not x
```

In the above example, `y` is `True`, but changing it to `y = x` make it `123` instead.

## Solution

Only suggest `x` when in a boolean context, and `bool(x)` everywhere else:

```python
y = not not x  # bool(x)

if not not x:  # x
    ...
elif not not x:  # x
    ...
elif f(not not x):  # f(bool(x))
    ...
```

I opened a similar issue on the [flake8-simplify](https://github.com/MartinThoma/flake8-simplify/issues/170) repo as well.

---

_Comment by @charliermarsh on 2023-03-05 00:36_

Oh interesting, thanks, I think we do this already (or something like it) for `SIM210`.

---

_Label `bug` added by @charliermarsh on 2023-03-05 00:36_

---

_Closed by @charliermarsh on 2023-05-08 23:38_

---

_Comment by @crimsonknave on 2024-08-06 17:46_

I'm getting flagged for SIM208 in the exact example listed in the description. It does not suggest I use `bool(x)` and instead suggests to use `x`.

Python version: 3.11.9
Ruff version: 0.5.6

---
