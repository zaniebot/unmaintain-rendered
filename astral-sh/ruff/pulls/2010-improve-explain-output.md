```yaml
number: 2010
title: Improve --explain output
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: better-explain
created_at: 2023-01-20T02:25:01Z
updated_at: 2023-01-20T03:08:01Z
url: https://github.com/astral-sh/ruff/pull/2010
synced_at: 2026-01-12T15:55:07Z
```

# Improve --explain output

---

_@not-my-profile_

Previous output for `ruff --explain E711`:

    E711 (pycodestyle): Comparison to `None` should be `cond is None`

New output:

    none-comparison

    Code: E711 (pycodestyle)

    Autofix is always available.

    Message formats:

    * Comparison to `None` should be `cond is None`
    * Comparison to `None` should be `cond is not None`

Please merge this after #2009 since the commit message assumes that the AsRef impl has been changed to kebab-case.

---

_Converted to draft by @not-my-profile on 2023-01-20 02:39_

---

_Marked ready for review by @not-my-profile on 2023-01-20 02:40_

---

_Merged by @charliermarsh on 2023-01-20 03:08_

---

_Closed by @charliermarsh on 2023-01-20 03:08_

---
