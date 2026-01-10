```yaml
number: 701
title: "Tell Rooster to ignore PRs labeled with `ci` when generating changelogs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - release
assignees: []
merged: true
base: main
head: ignore-ci-prs
created_at: 2025-06-25T11:19:42Z
updated_at: 2025-06-25T11:28:48Z
url: https://github.com/astral-sh/ty/pull/701
synced_at: 2026-01-10T02:34:10Z
```

# Tell Rooster to ignore PRs labeled with `ci` when generating changelogs

---

_Pull request opened by @AlexWaygood on 2025-06-25 11:19_

A few PRs showed up in the initial version of the changelog for https://github.com/astral-sh/ty/pull/700 because they were labeled with `ci` but not with `internal` or `testing`. We should tell Rooster to ignore PRs with this label, same as we do at Ruff: https://github.com/astral-sh/ruff/blob/c1fed55d51b34e1235f72c610fe4b1700ed76451/pyproject.toml#L110


---

_Label `release` added by @AlexWaygood on 2025-06-25 11:20_

---

_@sharkdp approved on 2025-06-25 11:27_

---

_Merged by @AlexWaygood on 2025-06-25 11:28_

---

_Closed by @AlexWaygood on 2025-06-25 11:28_

---

_Branch deleted on 2025-06-25 11:28_

---
