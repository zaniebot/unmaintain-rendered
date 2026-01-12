```yaml
number: 148
title: "Fix ruff's pyproject.toml section name in README.md"
type: pull_request
state: merged
author: Jackenmen
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2022-09-11T02:49:52Z
updated_at: 2022-09-11T15:43:11Z
url: https://github.com/astral-sh/ruff/pull/148
synced_at: 2026-01-12T15:55:04Z
```

# Fix ruff's pyproject.toml section name in README.md

---

_@Jackenmen_

That took me a bit to figure out why ruff isn't applying any of the settings I put in pyproject.toml :)

Side suggestion: make `-v` tell you more about settings loading - it currently just tells you that it found `pyproject.toml` but not whether, for example, it found ruff section *or* what settings it picked up.

---

_@charliermarsh approved on 2022-09-11 14:18_

Oof, thank you!

---

_Merged by @charliermarsh on 2022-09-11 14:18_

---

_Closed by @charliermarsh on 2022-09-11 14:18_

---

_Branch deleted on 2022-09-11 15:43_

---
