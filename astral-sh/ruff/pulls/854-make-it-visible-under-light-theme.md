```yaml
number: 854
title: Make it visible under light theme
type: pull_request
state: merged
author: kemingy
labels: []
assignees: []
merged: true
base: main
head: support_light_theme
created_at: 2022-11-21T15:09:50Z
updated_at: 2022-11-21T15:18:27Z
url: https://github.com/astral-sh/ruff/pull/854
synced_at: 2026-01-12T05:48:46Z
```

# Make it visible under light theme

---

_Pull request opened by @kemingy on 2022-11-21 15:09_

Signed-off-by: Keming <kemingy94@gmail.com>

* close #852

---

_@charliermarsh reviewed on 2022-11-21 15:14_

---

_Review comment by @charliermarsh on `src/message.rs`:60 on 2022-11-21 15:14_

Does the cyan work out ok for light themes? Should we have an entirely separate "theme"?

---

_@kemingy reviewed on 2022-11-21 15:16_

---

_Review comment by @kemingy on `src/message.rs`:60 on 2022-11-21 15:16_

Yes. Cyan works well.

For now, I think the default color (white on the dark theme and black on the light theme) looks good. Red and cyan should work for most of the themes.

---

_Merged by @charliermarsh on 2022-11-21 15:18_

---

_Closed by @charliermarsh on 2022-11-21 15:18_

---

_Branch deleted on 2022-11-21 15:18_

---
