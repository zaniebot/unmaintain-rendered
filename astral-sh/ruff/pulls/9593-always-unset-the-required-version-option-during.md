```yaml
number: 9593
title: "Always unset the `required-version` option during ecosystem checks"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - ci
assignees: []
merged: true
base: main
head: zb/eco-ignore-required-version
created_at: 2024-01-20T16:35:12Z
updated_at: 2024-01-21T05:08:09Z
url: https://github.com/astral-sh/ruff/pull/9593
synced_at: 2026-01-12T15:55:29Z
```

# Always unset the `required-version` option during ecosystem checks

---

_@zanieb_

Uses our existing configuration overrides to unset the `required-version` option in ecosystem projects during checks.

The downside to this approach, is we will now update the config file of _every_ project. This roughly normalizes the configuration file, as we don't preserve comments and such. We could instead do a more targeted approach applying this override to projects which we know use this setting 🤷‍♀️ 

---

_@charliermarsh approved on 2024-01-20 16:37_

---

_Label `internal` added by @charliermarsh on 2024-01-20 16:37_

---

_Label `ci` added by @zanieb on 2024-01-20 16:38_

---

_Comment by @github-actions[bot] on 2024-01-20 16:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-01-21 03:30_

I could imagine an alternative approach wherein we expose `--required-version` as a command-line argument and/or environment variable, and then set it to the current version.

---

_Comment by @zanieb on 2024-01-21 05:06_

There's already the infrastructure for overriding config file options for this purpose, I'd rather not change the public API without stronger justification.

---

_Merged by @zanieb on 2024-01-21 05:07_

---

_Closed by @zanieb on 2024-01-21 05:07_

---

_Branch deleted on 2024-01-21 05:07_

---

_Comment by @zanieb on 2024-01-21 05:08_

Oh also... the work Alex was doing on config overrides via the CLI would suffice for that approach. Seems nice to switch over once we have it.

---
