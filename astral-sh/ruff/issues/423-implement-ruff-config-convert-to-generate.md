```yaml
number: 423
title: "Implement `ruff config-convert` to generate `pyproject.toml` from Flake8 configs"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-10-13T22:14:23Z
updated_at: 2022-11-02T19:43:10Z
url: https://github.com/astral-sh/ruff/issues/423
synced_at: 2026-01-10T15:56:05Z
```

# Implement `ruff config-convert` to generate `pyproject.toml` from Flake8 configs

---

_Issue opened by @charliermarsh on 2022-10-13 22:14_

See: #414.

---

_Label `enhancement` added by @charliermarsh on 2022-10-13 22:14_

---

_Comment by @andersk on 2022-10-26 16:21_

This probably requires

- #325

---

_Comment by @charliermarsh on 2022-10-31 02:47_

@fsouza - FYI, I'm working on this now.

---

_Comment by @charliermarsh on 2022-11-01 22:08_

This now exists as [`flake8-to-ruff`](https://pypi.org/project/flake8-to-ruff/). Anyone willing to test it out and give feedback? Maybe @fsouza?

---

_Comment by @fsouza on 2022-11-01 22:54_

> This now exists as [`flake8-to-ruff`](https://pypi.org/project/flake8-to-ruff/). Anyone willing to test it out and give feedback? Maybe @fsouza?

I'll give it a try!

---

_Comment by @fsouza on 2022-11-02 02:22_

I left a comment in #527, it seems to be working, just failed with one setting (`max-line-length`).

---

_Comment by @charliermarsh on 2022-11-02 02:27_

Fixed in https://github.com/charliermarsh/ruff/pull/541.

---

_Closed by @charliermarsh on 2022-11-02 19:43_

---
