```yaml
number: 7618
title: Improvements to RUF15
type: issue
state: closed
author: hoxbro
labels:
  - rule
assignees: []
created_at: 2023-09-23T13:40:16Z
updated_at: 2023-10-08T14:49:47Z
url: https://github.com/astral-sh/ruff/issues/7618
synced_at: 2026-01-12T15:54:47Z
```

# Improvements to RUF15

---

_@hoxbro_

I have two (small) suggestions for improving [unnecessary-iterable-allocation-for-first-element (RUF015)](https://docs.astral.sh/ruff/rules/unnecessary-iterable-allocation-for-first-element/#unnecessary-iterable-allocation-for-first-element-ruf015)

1) Don't add `iter` to built-in functions that are already iterable
An example of this could be `list(zip([0]))[0]`, which right now is converted to `next(iter(zip([10])))`, and my suggestion would make it not add `iter` so it would be `next(zip([10]))`

2) Handle an unpacked list
This could be something like this `[*x][0]`, which should be handled like `list(x)[0]` and converts to `next(iter(x))`. I don't think this is used much in the wild, so it could be overkill to add it. 

I have a first already started implementing this [here](https://github.com/Hoxbro/ruff/pull/2). Though, as this is the first time I have written any Rust, it could likely be improved upon 😅

---

_Label `rule` added by @charliermarsh on 2023-09-23 16:01_

---

_Comment by @charliermarsh on 2023-09-23 16:02_

I'd like to support the former but would probably skip the latter -- or, at least, that could be a separate rule that converts `[*x]` to `list(x)`, rather than part of RUF015.

---

_Comment by @hoxbro on 2023-09-23 16:10_

With the latter my point was _not_ to do a conversion of `[*x]` to `list(x)`, but convert `[*x][0]`  to `next(iter(x))`.

 Can you assign me this?

---

_Assigned to @hoxbro by @charliermarsh on 2023-09-23 16:42_

---

_Closed by @charliermarsh on 2023-10-08 14:49_

---
