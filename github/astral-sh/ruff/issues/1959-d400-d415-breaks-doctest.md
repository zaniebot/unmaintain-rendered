---
number: 1959
title: D400 / D415 breaks doctest
type: issue
state: closed
author: spaceone
labels:
  - bug
  - docstring
assignees: []
created_at: 2023-01-18T15:28:05Z
updated_at: 2023-01-18T20:47:32Z
url: https://github.com/astral-sh/ruff/issues/1959
synced_at: 2026-01-07T13:12:14-06:00
---

# D400 / D415 breaks doctest

---

_Issue opened by @spaceone on 2023-01-18 15:28_

```python
#!/usr/bin/python3
def calculate_ipv6_reverse(network):
    """Return reversed network part of IPv4 network.
    >>> calculate_ipv6_reverse(False)
    'False'
    >>> calculate_ipv6_reverse(True)
    'True'
    """
    return str(network)
```

`ruff --isolated --select D --fix foo.py` turns that into:

```python
#!/usr/bin/python3
def calculate_ipv6_reverse(network):
    """Return reversed network part of IPv4 network.
    >>> calculate_ipv6_reverse(False)
    'False'
    >>> calculate_ipv6_reverse(True)
    'True'.
    """
    return str(network)
```

which then cause a failing doctest:
```
$ python3 -m doctest foo.py
**********************************************************************
File "foo.py", line 6, in foo.calculate_ipv6_reverse
Failed example:
    calculate_ipv6_reverse(True)
Expected:
    'True'.
Got:
    'True'
**********************************************************************
1 items had failures:
   1 of   2 in foo.calculate_ipv6_reverse
***Test Failed*** 1 failures.

```

---

_Label `bug` added by @charliermarsh on 2023-01-18 15:40_

---

_Label `docstring` added by @charliermarsh on 2023-01-18 15:40_

---

_Comment by @charliermarsh on 2023-01-18 16:41_

I think you should add a blank line between the summary and the doc tests. The [doctest docs](https://docs.python.org/3/library/doctest.html) do the same thing, and it's the safest way to avoid this false positive.

---

_Closed by @charliermarsh on 2023-01-18 16:41_

---

_Comment by @spaceone on 2023-01-18 20:22_

ok, I can adjust the new line.

This also happens for:

```
def calculate_ipv6_reverse(network):
    """
    >>> calculate_ipv6_reverse(False)
    'False'
    """
```

---

_Comment by @spaceone on 2023-01-18 20:47_

Maybe a new check which detects the missing blank line could be added?

---
