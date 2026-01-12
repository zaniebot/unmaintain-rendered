```yaml
number: 3159
title: "Add Autofix::is_enabled() to remove repeative patterns"
type: pull_request
state: merged
author: youknowone
labels: []
assignees: []
merged: true
base: main
head: autofix-is-enabled
created_at: 2023-02-23T02:37:01Z
updated_at: 2023-02-23T05:02:21Z
url: https://github.com/astral-sh/ruff/pull/3159
synced_at: 2026-01-12T04:39:44Z
```

# Add Autofix::is_enabled() to remove repeative patterns

---

_Pull request opened by @youknowone on 2023-02-23 02:37_

_No description provided._

---

_@charliermarsh reviewed on 2023-02-23 02:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/flags.rs`:10 on 2023-02-23 02:39_

Can / should this be `const`?

---

_Review comment by @youknowone on `crates/ruff/src/settings/flags.rs`:10 on 2023-02-23 03:08_

Oh,  it does. Thank you!
By the way, if you want to adapt more convenient ways to do this, I'd like to introduce a macro about it:

https://docs.rs/is-macro/latest/is_macro/derive.Is.html

---

_@youknowone reviewed on 2023-02-23 03:08_

---

_Review requested from @charliermarsh by @youknowone on 2023-02-23 03:14_

---

_@charliermarsh reviewed on 2023-02-23 03:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/flags.rs`:10 on 2023-02-23 03:18_

That looks reasonable. We do this a lot especially for flags. Let's maybe start with the various flags and such, and not touch the `ExprKind` and `StmtKind` checks for now.

---

_Merged by @charliermarsh on 2023-02-23 04:52_

---

_Closed by @charliermarsh on 2023-02-23 04:52_

---

_Branch deleted on 2023-02-23 05:02_

---
