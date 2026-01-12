```yaml
number: 2606
title: Make self-update an opt-in Cargo feature
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/self-update
created_at: 2024-03-22T04:10:00Z
updated_at: 2024-03-22T04:23:10Z
url: https://github.com/astral-sh/uv/pull/2606
synced_at: 2026-01-12T16:05:07Z
```

# Make self-update an opt-in Cargo feature

---

_@charliermarsh_

## Summary

Ensures that (e.g.) installs from conda-forge, Homebrew, and other distributions don't expose `uv self update` at all.

We'll still show `uv self update` for `pip install uv`, but it will fail with a good error. Removing the `uv self update` from `pip`-installed `uv` is more complicated, since we'd need to build separately for the installer vs. for PyPI.

Closes #2588.


---

_Label `release` added by @charliermarsh on 2024-03-22 04:10_

---

_Marked ready for review by @charliermarsh on 2024-03-22 04:10_

---

_@zanieb approved on 2024-03-22 04:15_

---

_Merged by @charliermarsh on 2024-03-22 04:23_

---

_Closed by @charliermarsh on 2024-03-22 04:23_

---

_Branch deleted on 2024-03-22 04:23_

---
