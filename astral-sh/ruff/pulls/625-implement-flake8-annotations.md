```yaml
number: 625
title: Implement flake8-annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/flake8-annotations
created_at: 2022-11-06T22:03:46Z
updated_at: 2022-11-06T22:25:50Z
url: https://github.com/astral-sh/ruff/pull/625
synced_at: 2026-01-12T15:55:05Z
```

# Implement flake8-annotations

---

_@charliermarsh_

This PR implements `flake8-annotations`. All checks are implemented, though I opted to take advantage of our (IMO) better detection of public and private methods to use single public vs. private rules, rather than private and protected rules based solely on function names.

I implemented a few configuration settings, but not all: `mypy_init_return`, `suppress_dummy_args`, `suppress_none_returning`.

Resolves #579.


---

_Merged by @charliermarsh on 2022-11-06 22:25_

---

_Closed by @charliermarsh on 2022-11-06 22:25_

---

_Branch deleted on 2022-11-06 22:25_

---
