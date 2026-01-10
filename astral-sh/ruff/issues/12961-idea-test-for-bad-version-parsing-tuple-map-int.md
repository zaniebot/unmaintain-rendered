```yaml
number: 12961
title: "Idea: Test for bad version parsing `tuple(map(int, __version__)))`"
type: issue
state: closed
author: randolf-scholz
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-08-18T09:52:47Z
updated_at: 2024-11-18T13:43:25Z
url: https://github.com/astral-sh/ruff/issues/12961
synced_at: 2026-01-10T11:09:54Z
```

# Idea: Test for bad version parsing `tuple(map(int, __version__)))`

---

_Issue opened by @randolf-scholz on 2024-08-18 09:52_

Inspired by a [recent bug in pycharm](https://youtrack.jetbrains.com/issue/PY-75115), caused by bad version parsing that went undetected until `matplotlib`, [for the first time in 20 years](https://pypi.org/project/matplotlib/#history), published a non-pre-release version with a postfix (`3.9.1.post1` on Aug 7th).

## What it does
 
Checks for the presence of `tuple(map(int, obj.__version__.split(".")))` and equivalent variants thereof.

## Why this is bad

This breaks if the package uses a postfix that is not convertible to integer, such as `matplotlib==3.9.1.post1`.

## Use instead

Version string should be parsed using the PEP440 specification.

- PyPA's `packaging` provides the [packaging.version](https://packaging.pypa.io/en/latest/version.html) utility.
- PEP440 provides a [canonical regex](https://peps.python.org/pep-0440/#appendix-b-parsing-version-strings-with-regular-expressions) if one wants to avoid additional dependencies.





---

_Comment by @randolf-scholz on 2024-08-18 09:57_

A quick [GitHub search](https://github.com/search?q=%2Ftuple%5C%28map%5C%28int%2C+%5Cw%2B%5C.__version__.split%5C%28%5B%22%27%5D%5C.%5B%22%27%5D%5C%29%5C%29%5C%29%2F+language%3APython&type=code) shows that this vulnerability is present in many packages.

---

_Label `rule` added by @charliermarsh on 2024-08-18 22:54_

---

_Comment by @charliermarsh on 2024-08-18 22:54_

I'm in favor of this.

---

_Label `accepted` added by @charliermarsh on 2024-08-18 22:54_

---

_Closed by @AlexWaygood on 2024-11-18 13:43_

---
