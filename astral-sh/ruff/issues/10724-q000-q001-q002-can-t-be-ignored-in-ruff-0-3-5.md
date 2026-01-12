```yaml
number: 10724
title: "`Q000`, `Q001`, `Q002` can't be ignored in Ruff 0.3.5"
type: issue
state: closed
author: hinohi
labels:
  - bug
assignees: []
created_at: 2024-04-02T01:19:59Z
updated_at: 2024-04-04T08:08:53Z
url: https://github.com/astral-sh/ruff/issues/10724
synced_at: 2026-01-12T15:54:50Z
```

# `Q000`, `Q001`, `Q002` can't be ignored in Ruff 0.3.5

---

_@hinohi_

In Ruff 0.3.5, if specifying to ignore one or two of the Q000, Q001, Q002 rules, they are not ignored.
However, if specifying to ignore all three of those rules, then it works as expected.

Reproduction code:

```python
'''
bad docsting
'''
a = 'single'
b = '''
bad multi line
'''
```

Running:

```
ruff check . --isolated --select Q --ignore Q000
ruff check . --isolated --select Q --ignore Q001
ruff check . --isolated --select Q --ignore Q002
ruff check . --isolated --select Q --ignore Q000,Q001
ruff check . --isolated --select Q --ignore Q000,Q002
ruff check . --isolated --select Q --ignore Q001,Q002
```

Output  is:

```
a.py:1:1: Q002 [*] Single quote docstring found but double quotes preferred
a.py:4:5: Q000 Single quotes found but double quotes preferred
a.py:5:5: Q001 [*] Single quote multiline found but double quotes preferred
Found 3 errors.
[*] 2 fixable with the `--fix` option.
```

If specifying to ignore all three of those rules, then it works as expected:

```
$ ruff check . --isolated --select Q --ignore Q000,Q001,Q002
All checks passed!
```

* In 0.3.4, it correctly shows `All checks passed!`.
* I've tested this with Python versions 3.12.0 and 3.9.18, and the behavior is the same on both.
* I've observed this on both macOS 13.6 and Ubuntu 22.04.
* The issue persists whether configuring via the pyproject.toml file or command line arguments.

Please let me know if you need any clarification or have additional details to provide.

---

_Renamed from "ignore `Q000x` not working in Ruff 0.3.5" to "ignore `Q00x` not working in Ruff 0.3.5" by @hinohi on 2024-04-02 01:23_

---

_Comment by @averykhoo on 2024-04-02 01:58_

+1, facing the same issue

my `ruff.toml` includes:
```toml
[lint]
select = [
    ...
    "Q", # flake8-quotes
    ...
]
ignore = [
    ...
    "Q000", # don't be pedantic about 'string' / "string"
    "Q001", # don't be pedantic about '''string''' / """string"""
    ...
]
```

---

_Comment by @averykhoo on 2024-04-02 02:21_

was testing the config, the following flags `Q000` when it clearly shouldn't:
```toml
[lint]
select = [
    "Q002", # flake8-quotes:bad-quotes-docstring
]
```


but the following doesn't flag `Q000`
```toml
[lint]
select = [
    ...
    #"Q", # flake8-quotes
    "Q000",
    "Q001",
    "Q003",
    "Q004",
    ...
]
ignore = [
    ...
    "Q000", # don't be pedantic about 'string' / "string"
    "Q001", # don't be pedantic about '''string''' / """string"""
    ...
]
```
so it doesn't seem to be a problem with ignore, just with `Q002` specifically

hope this helps you narrow down the issue!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 02:54_

---

_Label `bug` added by @charliermarsh on 2024-04-02 02:54_

---

_Comment by @charliermarsh on 2024-04-02 02:54_

I think it's just an oversight in where we apply the ignores for those rules. I'll fix it now. Thanks!

---

_Closed by @charliermarsh on 2024-04-02 03:21_

---

_Comment by @charliermarsh on 2024-04-02 03:44_

Fixed in the next release, sorry about that.

---

_Renamed from "ignore `Q00x` not working in Ruff 0.3.5" to "`Q000`, `Q001`, `Q002` can't be ignored in Ruff 0.3.5" by @AlexWaygood on 2024-04-04 08:08_

---
