---
number: 9380
title: Reformatted code quality in example is quite poor
type: issue
state: closed
author: stefanoborini
labels:
  - question
assignees: []
created_at: 2024-01-03T13:30:47Z
updated_at: 2024-01-03T14:47:45Z
url: https://github.com/astral-sh/ruff/issues/9380
synced_at: 2026-01-10T01:22:49Z
---

# Reformatted code quality in example is quite poor

---

_Issue opened by @stefanoborini on 2024-01-03 13:30_

From the example:

```
With the above configuration, this code:


def f(x):
    '''
    Something about `f`. And an example:

    .. code-block:: python

        foo, bar, quux = this_is_a_long_line(lion, hippo, lemur, bear)
    '''
    pass
... will be reformatted (assuming the rest of the options are set to their defaults) as:


def f(x):
    """
    Something about `f`. And an example:

    .. code-block:: python

        (
            foo,
            bar,
            quux,
        ) = this_is_a_long_line(
            lion,
            hippo,
            lemur,
            bear,
        )
    """
    pass
```

I fail to see a circumstance where the second form is more pleasant and readable than the first. Is it possible to prevent such reorganisation configuration-wise?


---

_Comment by @charliermarsh on 2024-01-03 14:46_

Hey -- it's hard to really take action on this issue, but the purpose of that example is to demonstrate the line-length rules for formatting code within docstrings, rather than provide any commentary on the style of the reformatted code. The style, though, is consistent with Black, and so within the formatter's goals.

---

_Closed by @charliermarsh on 2024-01-03 14:46_

---

_Label `question` added by @charliermarsh on 2024-01-03 14:46_

---

_Comment by @charliermarsh on 2024-01-03 14:46_

The short answer is no, apart from increasing the line length to prevent Ruff from wrapping lines in those cases. In that example, [docstring-code-line-length](https://docs.astral.sh/ruff/settings/#format-docstring-code-line-length) is being set to 20 which is an extremely low value just for the purpose of demonstration.

---
