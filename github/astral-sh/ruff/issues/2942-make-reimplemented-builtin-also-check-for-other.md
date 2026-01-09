---
number: 2942
title: "Make `reimplemented-builtin` also check for other builtins"
type: issue
state: open
author: not-my-profile
labels:
  - rule
assignees: []
created_at: 2023-02-15T21:39:21Z
updated_at: 2023-02-18T12:29:14Z
url: https://github.com/astral-sh/ruff/issues/2942
synced_at: 2026-01-07T13:12:14-06:00
---

# Make `reimplemented-builtin` also check for other builtins

---

_Issue opened by @not-my-profile on 2023-02-15 21:39_

`reimplemented-builtin` currently only checks for `any` and `all`.

Looking at the [list of builtins](https://docs.python.org/3/library/functions.html#built-in-funcs), I think additionally checking for the following would make sense:

* [ ] abs
* [ ] enumerate
* [ ] filter
* [ ] len
* [ ] map
* [ ] max
* [ ] min
* [ ] pow
* [ ] round
* [ ] sum
* [ ] zip

---

_Referenced in [astral-sh/ruff#2903](../../astral-sh/ruff/pulls/2903.md) on 2023-02-15 21:40_

---

_Label `rule` added by @charliermarsh on 2023-02-15 21:55_

---
