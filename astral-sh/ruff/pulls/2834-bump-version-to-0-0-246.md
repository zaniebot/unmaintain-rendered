```yaml
number: 2834
title: Bump version to 0.0.246
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2023-02-12T23:34:36Z
updated_at: 2023-02-13T02:23:15Z
url: https://github.com/astral-sh/ruff/pull/2834
synced_at: 2026-01-12T15:55:11Z
```

# Bump version to 0.0.246

---

_@charliermarsh_

_No description provided._

---

_Merged by @charliermarsh on 2023-02-12 23:39_

---

_Closed by @charliermarsh on 2023-02-12 23:39_

---

_Branch deleted on 2023-02-12 23:39_

---

_Comment by @AA-Turner on 2023-02-13 01:12_

Hi Charlie,

[PyPI](https://pypi.org/project/ruff/) and the [GitHub releases](https://github.com/charliermarsh/ruff/releases) feature aren't showing version 0.0.246 -- is this expected? (I don't know how long release builds etc take to run).

A

---

_Comment by @charliermarsh on 2023-02-13 01:22_

Yeah I haven’t created the release yet — only bumped the version. I’ll likely do it later tonight :)

---

_Comment by @AA-Turner on 2023-02-13 02:16_

Ahh fantastic---thank you so much!

A

---

_Comment by @charliermarsh on 2023-02-13 02:23_

Just a heads up in advance: we added `SIM114` (same-body on `if` statement branches) in this release, so Sphinx has some new violations on `0.0.246` (just checked myself). You may want to add to your ignore when you upgrade (or enforce them, your call of course :)).

---
