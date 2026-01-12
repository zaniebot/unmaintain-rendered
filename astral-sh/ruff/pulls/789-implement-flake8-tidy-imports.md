```yaml
number: 789
title: Implement flake8-tidy-imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tidy
created_at: 2022-11-17T16:27:16Z
updated_at: 2022-11-17T16:44:08Z
url: https://github.com/astral-sh/ruff/pull/789
synced_at: 2026-01-12T05:48:45Z
```

# Implement flake8-tidy-imports

---

_Pull request opened by @charliermarsh on 2022-11-17 16:27_

This PR implements `I252` (banned relative imports). The other rules will come in a separate PR.

The one difference from `flake8-tidy-imports` is that I've changed the configuration values from `parents` and `true` to `"parents"` and `"all"`, since TOML has types unlike INI.

This was referenced in #283.


---

_Merged by @charliermarsh on 2022-11-17 16:44_

---

_Closed by @charliermarsh on 2022-11-17 16:44_

---

_Branch deleted on 2022-11-17 16:44_

---
