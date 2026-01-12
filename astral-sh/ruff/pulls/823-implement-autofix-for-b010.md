```yaml
number: 823
title: Implement autofix for B010
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: autofix-B010
created_at: 2022-11-20T08:06:23Z
updated_at: 2022-11-20T15:14:59Z
url: https://github.com/astral-sh/ruff/pull/823
synced_at: 2026-01-12T15:55:05Z
```

# Implement autofix for B010

---

_@harupy_

#389 

---

_Merged by @charliermarsh on 2022-11-20 15:14_

---

_Closed by @charliermarsh on 2022-11-20 15:14_

---

_@charliermarsh reviewed on 2022-11-20 15:14_

---

_Review comment by @charliermarsh on `src/checks.rs`:2079 on 2022-11-20 15:14_

@harupy - Small thing -- these need to be added to this `fixable` list once they support autofix. That way, they'll automatically update properly when you run `cargo dev generate-rules-table`.

---
