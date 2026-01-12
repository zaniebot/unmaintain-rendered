```yaml
number: 4365
title: Use target name in hardcoded-password diagnostics
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/keys
created_at: 2023-05-11T02:38:31Z
updated_at: 2023-05-11T02:54:29Z
url: https://github.com/astral-sh/ruff/pull/4365
synced_at: 2026-01-12T03:56:39Z
```

# Use target name in hardcoded-password diagnostics

---

_Pull request opened by @charliermarsh on 2023-05-11 02:38_

#3511 mentioned some false positives for Bandit's "hardcoded password" rules. I think the confusion is that those rules test the _target_ to determine whether a string might be a hardcoded password, but the diagnostics report the _value_. So, e.g., if you have `api_token = "foo"`, it tells you that there's a `Possible hardcoded password: "foo"`, rather than that the cause is assigning to `api_token`.

There are still some false positives (e.g., in torchmetrics, they have `split_token`, which gets flagged here), but we can only avoid those via special-casing. Making the diagnostics clearer at least provides the proper context.

Closes #3511.


---

_Merged by @charliermarsh on 2023-05-11 02:54_

---

_Closed by @charliermarsh on 2023-05-11 02:54_

---

_Branch deleted on 2023-05-11 02:54_

---
