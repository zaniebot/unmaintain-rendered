```yaml
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
synced_at: 2026-01-12T15:54:43Z
```

# Make `reimplemented-builtin` also check for other builtins

---

_@not-my-profile_

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

_Label `rule` added by @charliermarsh on 2023-02-15 21:55_

---
