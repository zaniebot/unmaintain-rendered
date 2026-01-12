```yaml
number: 915
title: Automatically wrap certain simple docstrings
type: pull_request
state: closed
author: kurtbuilds
labels: []
assignees: []
base: main
head: ks/wrap-props
created_at: 2022-11-26T18:15:00Z
updated_at: 2022-12-11T15:24:01Z
url: https://github.com/astral-sh/ruff/pull/915
synced_at: 2026-01-12T15:55:05Z
```

# Automatically wrap certain simple docstrings

---

_@kurtbuilds_

Automatic line length wrapping is a huge can of worms.

This PR starts to address one isolated case where we can automatically solve line length wrapping, namely for some docstrings. Example:

```python
class Account:
    address: str = None
    """Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."""
```

is automatically corrected to

```python
class Account:
    address: str = None
    """Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
    eiusmod tempor incididunt ut labore et dolore magna aliqua."""
```

I'm guessing there's more checking that needs to happen (e.g. this might wrap things that we don't want, like reStructuredText values). This PR serves as a starting point.

---

_Comment by @charliermarsh on 2022-11-29 02:03_

I'm a little hesitant on this fix because splitting the first line across multiple lines creates some other lint errors 🤔 Namely, the first line is expected to be a single line, followed potentially by a multi-line body.


---

_Comment by @charliermarsh on 2022-12-11 15:24_

Going to close for now but this is something I'd be open to exploring. E.g., it _could_ be nice to automatically wrap docstring bodies apart from the summary line.


---

_Closed by @charliermarsh on 2022-12-11 15:24_

---
