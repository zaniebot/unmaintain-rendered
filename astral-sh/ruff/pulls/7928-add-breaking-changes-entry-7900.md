```yaml
number: 7928
title: "Add breaking changes entry #7900"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/breaking-ruleset
created_at: 2023-10-12T13:58:24Z
updated_at: 2023-10-12T16:01:52Z
url: https://github.com/astral-sh/ruff/pull/7928
synced_at: 2026-01-12T15:55:25Z
```

# Add breaking changes entry #7900

---

_@zanieb_

_No description provided._

---

_Renamed from "Add breaking changes entry for https://github.com/astral-sh/ruff/pull/7900" to "Add breaking changes entry #7900" by @zanieb on 2023-10-12 14:02_

---

_@charliermarsh reviewed on 2023-10-12 15:46_

---

_Review comment by @charliermarsh on `BREAKING_CHANGES.md`:11 on 2023-10-12 15:46_

I tweaked the last two sentences; feel free to take or leave:

```text
Previously, Ruff enabled all implemented rules in Pycodestyle (`E`) by default. Ruff now only includes the
Pycodestyle prefixes `E4`, `E7`, and `E9` to exclude rules that conflict with automatic formatters. Consequently,
the stable rule set no longer includes `line-too-long` (`E501`) and `mixed-spaces-and-tabs` (`E101`). Other
excluded Pycodestyle rules include whitespace enforcement in `E1` and `E2`; these rules are currently in preview,
and so are already omitted by default.

This change only affects those using Ruff under its default rule set. Users that include `E` in their `select` will
experience no change in behavior.
```

---

_Review comment by @charliermarsh on `BREAKING_CHANGES.md`:5 on 2023-10-12 15:46_

Nit: `Remove formatter-conflicting rules from the default rule set`?

---

_@charliermarsh reviewed on 2023-10-12 15:46_

---

_@charliermarsh approved on 2023-10-12 15:47_

---

_@zanieb reviewed on 2023-10-12 15:48_

---

_Review comment by @zanieb on `BREAKING_CHANGES.md`:5 on 2023-10-12 15:48_

Sure
```suggestion
### Remove formatter-conflicting rules from the default rule set
```

---

_Merged by @zanieb on 2023-10-12 16:01_

---

_Closed by @zanieb on 2023-10-12 16:01_

---

_Branch deleted on 2023-10-12 16:01_

---
