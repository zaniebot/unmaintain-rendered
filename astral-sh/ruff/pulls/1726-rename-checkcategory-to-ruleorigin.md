```yaml
number: 1726
title: "Rename `CheckCategory` to `RuleOrigin`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/origin
created_at: 2023-01-07T23:08:14Z
updated_at: 2023-01-08T22:50:19Z
url: https://github.com/astral-sh/ruff/pull/1726
synced_at: 2026-01-12T05:36:32Z
```

# Rename `CheckCategory` to `RuleOrigin`

---

_Pull request opened by @charliermarsh on 2023-01-07 23:08_

_No description provided._

---

_Comment by @charliermarsh on 2023-01-07 23:08_

\cc @not-my-profile 

---

_Comment by @not-my-profile on 2023-01-08 03:35_

I'd prefer `RuleOrigin` to clarify what origin is meant.

---

_@not-my-profile reviewed on 2023-01-08 03:37_

---

_Review comment by @not-my-profile on `src/registry.rs`:892 on 2023-01-08 03:37_

This function should be renamed as well to `origin`.

---

_Comment by @not-my-profile on 2023-01-08 04:32_

Not related to this PR but why do the URLs in `CheckCategory::url` contain the version of the package? I don't think we actually keep track of which version our rules are based on, do we?

---

_Comment by @charliermarsh on 2023-01-08 21:08_

That's the version at the time of porting, so it represents the version that was implemented in Ruff. (If those plugins evolve in future versions, we may deviate depending on how they change.)

---

_Renamed from "Rename `CheckCategory` to `Origin`" to "Rename `CheckCategory` to `RuleOrigin`" by @charliermarsh on 2023-01-08 22:50_

---

_Merged by @charliermarsh on 2023-01-08 22:50_

---

_Closed by @charliermarsh on 2023-01-08 22:50_

---

_Branch deleted on 2023-01-08 22:50_

---
