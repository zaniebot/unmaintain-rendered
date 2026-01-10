```yaml
number: 1297
title: "`# noqa` not respected in docstrings."
type: issue
state: closed
author: pawelrubin
labels: []
assignees: []
created_at: 2022-12-20T10:06:06Z
updated_at: 2022-12-21T09:20:08Z
url: https://github.com/astral-sh/ruff/issues/1297
synced_at: 2026-01-10T12:05:25Z
```

# `# noqa` not respected in docstrings.

---

_Issue opened by @pawelrubin on 2022-12-20 10:06_

The `# noqa` directive is not respected in docstrings.

### Steps to reproduce

Run `ruff` on the following snippet

```python
"""
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt # noqa
"""


def foo():
    """
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt # noqa
    """


class Bar:
    """
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt # noqa
    """
```

### Expected result
Since each line longer than 88 characters end with a `# noqa`, there should be no errors.

### Actual result
```
Found 3 error(s).
snippet.py:2:1: E501 Line too long (96 > 88 characters)
snippet.py:8:1: E501 Line too long (100 > 88 characters)
snippet.py:14:1: E501 Line too long (100 > 88 characters) 
```

---

_Comment by @charliermarsh on 2022-12-20 16:08_

For multi-line strings, noqa comments are required to be placed on the closing line, rather than in the string body itself.

Can you try moving the noqa to just after the closing quotes, and report back?

---

_Comment by @charliermarsh on 2022-12-20 17:56_

See the [README](https://github.com/charliermarsh/ruff#ignoring-errors) for an example, but let me know if you continue to see issues!

---

_Closed by @charliermarsh on 2022-12-20 17:56_

---

_Comment by @pawelrubin on 2022-12-21 09:20_

Hi @charliermarsh. I missed the note in the README. Moving the `# noqa` directive to the closing quote solves the issue. Thank you for the quick response. 

---
