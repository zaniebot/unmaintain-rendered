```yaml
number: 962
title: "Consider adding `keep-runtime-typing` or equivalent option for `pyupgrade` rules"
type: issue
state: closed
author: layday
labels:
  - configuration
assignees: []
created_at: 2022-11-29T20:22:29Z
updated_at: 2022-11-30T01:05:33Z
url: https://github.com/astral-sh/ruff/issues/962
synced_at: 2026-01-10T12:09:58Z
```

# Consider adding `keep-runtime-typing` or equivalent option for `pyupgrade` rules

---

_Issue opened by @layday on 2022-11-29 20:22_

pyupgrade allows opting out of upgrading `typing` aliases of built-in collections and `typing.Union`s if they are used at runtime (e.g. by libraries such as Pydantic and cattrs) in Python versions in which they are unsupported if not stringified (< 3.9 for the former and < 3.10 for the latter).  It would be great if ruff could provide an equivalent option so that we are able to adopt the new syntax upon upgrading Python, without having to ignore the rules (and later forget to unignore them).

---

_Comment by @charliermarsh on 2022-11-29 20:42_

Can do!

---

_Label `enhancement` added by @charliermarsh on 2022-11-29 20:43_

---

_Label `configuration` added by @charliermarsh on 2022-11-29 20:43_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-30 00:33_

---

_Comment by @charliermarsh on 2022-11-30 01:05_

Added in https://github.com/charliermarsh/ruff/pull/965. I mirrored pyupgrade's behavior, which is that if you target Python 3.9, we still make those upgrades even if you passed in `--keep-runtime-typing` -- so that setting is only applicable when `--target-version` is 3.7 or 3.8, and you've imported `from __future__ import annotations` .

---

_Closed by @charliermarsh on 2022-11-30 01:05_

---
