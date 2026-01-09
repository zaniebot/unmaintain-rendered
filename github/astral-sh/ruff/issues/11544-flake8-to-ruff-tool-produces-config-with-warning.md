---
number: 11544
title: flake8-to-ruff tool produces config with warning
type: issue
state: closed
author: dwreeves
labels:
  - documentation
assignees: []
created_at: 2024-05-26T15:33:03Z
updated_at: 2024-05-26T23:39:32Z
url: https://github.com/astral-sh/ruff/issues/11544
synced_at: 2026-01-07T13:12:15-06:00
---

# flake8-to-ruff tool produces config with warning

---

_Issue opened by @dwreeves on 2024-05-26 15:33_

Input:

```shell
echo -e "[flake8]\nmax-line-length = 100" > .flake8
pip install flake8-to-ruff
flake8-to-ruff .flake8    
```

Output:

```toml
[tool.ruff]
ignore = []
line-length = 100
select = [
    "E",
    "F",
    "W",
]
```

Using this config produces the following warning:

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
```

---

_Comment by @charliermarsh on 2024-05-26 17:29_

Thank you! Unfortunately we no longer support `flake8-to-ruff` -- we removed references to it from our own docs and removed it from Ruff itself. I'll try to add a note to `flake8-to-ruff` itself; that may require me to release, in which case, we'll try to fix it too.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-26 18:12_

---

_Comment by @dwreeves on 2024-05-26 18:12_

Ah, that makes sense. I was wondering where it was in the code.

The warning is really minor and probably isn't worth making a code change over, since you're a busy guy and resurfacing old code is annoying. My $0.02 is, a more important change to the package, if you want to update the README would just be a note at the top of the README something like this:

```markdown
> **⚠️ Warning: `flake8-to-ruff` is no longer supported.** The last Ruff version supported by flake8-to-ruff was v0.0.233. You may experience warnings or breakage using this tool for later versions of Ruff.
```

And this captures not only the aforementioned warning but all other warnings.

v0.0.233 was released Jan 24, 2023: https://github.com/astral-sh/ruff/releases/tag/v0.0.233 which is the release of the PyPI project https://pypi.org/project/flake8-to-ruff/, hence why I am using that version. Although you may have a better idea of what version is better to say is supported.

---

_Comment by @charliermarsh on 2024-05-26 18:23_

Appreciate it! 👍 Yeah I think I need to do a release to add that warning anyway, so was considering just fixing it while I was going through the effort.

---

_Comment by @charliermarsh on 2024-05-26 18:23_

But I'll see how much trouble it causes me.

---

_Label `documentation` added by @charliermarsh on 2024-05-26 19:06_

---

_Comment by @charliermarsh on 2024-05-26 23:24_

Added the warning!

---

_Closed by @charliermarsh on 2024-05-26 23:24_

---

_Comment by @dwreeves on 2024-05-26 23:39_

half a day turnaround on a goofy and mostly inconsequential request on a Sunday, that's why you're the GOAT

---
