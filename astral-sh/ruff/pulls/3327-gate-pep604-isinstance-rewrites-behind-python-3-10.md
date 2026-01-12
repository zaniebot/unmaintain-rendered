```yaml
number: 3327
title: Gate PEP604 isinstance rewrites behind Python 3.10+
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/gate
created_at: 2023-03-03T22:08:51Z
updated_at: 2023-03-03T22:24:41Z
url: https://github.com/astral-sh/ruff/pull/3327
synced_at: 2026-01-12T15:55:12Z
```

# Gate PEP604 isinstance rewrites behind Python 3.10+

---

_@charliermarsh_

Closes #3326.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:2635 on 2023-03-03 22:09_

This flag is redundant, but it's already redundant for the other PEP 604 rule (since it's equivalent to turning off the rule), so I think it makes sense to be consistent.

---

_@charliermarsh reviewed on 2023-03-03 22:09_

---

_Review comment by @andersk on `crates/ruff/src/rules/pyupgrade/settings.rs`:31 on 2023-03-03 22:17_

An `isinstance` call is not an annotation, isn’t affected by `from __future__ import annotations`, and isn’t visible to `typing.get_type_hints` (the upstream motivation for this flag: asottile/pyupgrade#387), so I’m not sure the same logic applies.

---

_@andersk reviewed on 2023-03-03 22:17_

---

_Merged by @charliermarsh on 2023-03-03 22:22_

---

_Closed by @charliermarsh on 2023-03-03 22:22_

---

_Branch deleted on 2023-03-03 22:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/settings.rs`:31 on 2023-03-03 22:24_

Don't feel strongly! I thought it could feel inconsistent or surprising to users but your argument makes sense.

https://github.com/charliermarsh/ruff/pull/3328

---

_@charliermarsh reviewed on 2023-03-03 22:24_

---
