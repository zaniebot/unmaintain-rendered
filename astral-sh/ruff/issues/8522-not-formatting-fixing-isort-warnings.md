```yaml
number: 8522
title: Not formatting/fixing isort warnings?
type: issue
state: closed
author: vsalvino
labels: []
assignees: []
created_at: 2023-11-06T18:51:00Z
updated_at: 2023-11-06T19:00:32Z
url: https://github.com/astral-sh/ruff/issues/8522
synced_at: 2026-01-12T15:54:48Z
```

# Not formatting/fixing isort warnings?

---

_@vsalvino_

Hello - testing out ruff, great work here!

One issue that is perplexing me is that ruff does not seem to do any isort formatting or fixes, only linting. Is this correct? Or do I have it misconfigured somehow? Example:

```python
# File name: test.py
import os, typing


def check_files(paths: typing.List[str]):
    for p in paths:
        if not os.path.exists(p):
            return False
    return True
```

Tried the following commands:

```
$ ruff check test.py
test.py:1:1: E401 Multiple imports on one line
Found 1 error.
```

```
$ ruff check --fix test.py
test.py:1:1: E401 Multiple imports on one line
Found 1 error.
```

```
$ ruff format .\test.py
1 file left unchanged
```

I have tried using without any custom settings, and also with enabling isort `force-single-line` in pyproject.toml (which is the rule I'm most interested in). Same results either way.

Ruff version 0.1.4

---

_Comment by @charliermarsh on 2023-11-06 18:53_

So the isort rules aren't enabled by default. You can enable them by adding something like this to your `pyproject.toml`:

```toml
[tool.ruff.lint]
extend-select = ["I"]
```

---

_Comment by @charliermarsh on 2023-11-06 18:54_

With that, you should see isort appears appear when you run `ruff check`, and should see isort formatting applied when you run `ruff check --fix`.

---

_Comment by @vsalvino on 2023-11-06 19:00_

Perfect! Thank you for the lightning-fast response. Seems to be working now, on to more configuration!

---

_Closed by @vsalvino on 2023-11-06 19:00_

---
