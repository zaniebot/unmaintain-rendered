---
number: 9451
title: "Request: Remove sequential blank lines in docstrings"
type: issue
state: open
author: stinodego
labels:
  - rule
  - docstring
assignees: []
created_at: 2024-01-10T00:20:15Z
updated_at: 2024-02-07T19:28:47Z
url: https://github.com/astral-sh/ruff/issues/9451
synced_at: 2026-01-07T13:12:15-06:00
---

# Request: Remove sequential blank lines in docstrings

---

_Issue opened by @stinodego on 2024-01-10 00:20_

I'd like a way to get rid of extraneous whitespace in docstrings.

Here's an example of a docstring with way too much whitespace:

```python
def cool_function(x: int) -> None:
    """
    Do nothing.

    Parameters
    ----------
    x
        Description of x.



    Notes
    -----
    Hello.


    There.


    """

```

I'd like ruff to format this down to:

```python
def cool_function(x: int) -> None:
    """
    Do nothing.

    Parameters
    ----------
    x
        Description of x.

    Notes
    -----
    Hello.

    There.

    """
```

On a related note, there is currently no way to make sure there is _no_ blank line after the last section (the opposite of `D413`). This would be a useful lint to have!

---

_Label `docstring` added by @charliermarsh on 2024-01-10 19:43_

---

_Comment by @charliermarsh on 2024-01-10 19:44_

This looks useful.

---

_Label `rule` added by @charliermarsh on 2024-01-10 19:44_

---

_Comment by @MichaReiser on 2024-01-11 07:28_

@stinodego would you expect this to happen as part of a lint rule or are you asking for docstring formatting?

---

_Comment by @stinodego on 2024-01-11 07:31_

I honestly don't really know what the difference is between the two. Either is fine with me!

---

_Comment by @BrentWilkins on 2024-02-06 22:45_

This looks related to a change in [this PR](https://github.com/astral-sh/ruff/pull/9654). Ruff is calling this docstring bad:

```python
"""Make a summary line.

Note:
----
  Per the code comment the next two lines are blank. "// The first blank line is the line containing the closing
      triple quotes, so we need at least two."

"""
```

> Missing blank line after last section ("Note") Ruff([D413](https://docs.astral.sh/ruff/rules/blank-line-after-last-section))

I can't see what's different between what I have and the `Use instead:` section of [the documentation](https://docs.astral.sh/ruff/rules/blank-line-after-last-section/).

I can make Ruff happy by adding excessive blank lines:

```python
"""Make a summary line.

Note:
----
  Per the code comment the next two lines are blank. "// The first blank line is the line containing the closing
      triple quotes, so we need at least two."


"""
```

---

_Comment by @charliermarsh on 2024-02-06 22:50_

Seems like a bug in D413 (which was incorrectly implemented before that PR, and never really triggered), but I don't think it's related to this issue per se?

---

_Comment by @BrentWilkins on 2024-02-07 19:28_

Only in as much as this issue is to remove excessive whitespace like that bug is enforcing. I'll create a new issue for that. Thanks.

---

_Referenced in [astral-sh/ruff#13284](../../astral-sh/ruff/issues/13284.md) on 2024-09-08 17:01_

---

_Referenced in [astral-sh/ruff#11318](../../astral-sh/ruff/issues/11318.md) on 2025-05-10 13:06_

---
