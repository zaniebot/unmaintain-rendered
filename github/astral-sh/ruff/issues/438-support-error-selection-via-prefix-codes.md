---
number: 438
title: Support error selection via prefix codes
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-10-15T21:16:32Z
updated_at: 2022-10-18T21:19:14Z
url: https://github.com/astral-sh/ruff/issues/438
synced_at: 2026-01-07T13:12:14-06:00
---

# Support error selection via prefix codes

---

_Issue opened by @charliermarsh on 2022-10-15 21:16_

E.g., you should be able to `--select D` to enable all of the `pydocstyle` rules.


---

_Label `enhancement` added by @charliermarsh on 2022-10-15 21:16_

---

_Comment by @matteosantama on 2022-10-18 15:14_

My concern with this is that `numpy` and `google` convention are surely the most common `pydocstyle` settings, and I'm not sure if `--select D` would make using those two any easier.

For example, to turn on `convention=numpy`, would you do `--select D --exclude ...` and list out all the codes you don't want? But then you start running into precedence issues, ie. should `select` or `exclude` take precedence?

---

_Comment by @charliermarsh on 2022-10-18 21:17_

Alternatively, we could support a `--docstring-convention=...` setting that effectively adds the right codes to `--select`? Also interesting could be something more general that looks more like ESLint's system of "configs" (where each "config" turns on a set of error codes).

> But then you start running into precedence issues, ie. should `select` or `exclude` take precedence?

(Yeah, the point totally stands but FWIW we've already defined this such that `--ignore` takes precedence over `--select`.)


---

_Comment by @charliermarsh on 2022-10-18 21:19_

Oh, I'm actually going to close this as a duplicate of #325. That's my bad. We can continue discussion in there.

---

_Closed by @charliermarsh on 2022-10-18 21:19_

---
