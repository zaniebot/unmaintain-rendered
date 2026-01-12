```yaml
number: 10729
title: Accept non-aliased (but correct) import in unconventional-import-alias
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/inc
created_at: 2024-04-02T03:34:13Z
updated_at: 2024-04-02T03:47:20Z
url: https://github.com/astral-sh/ruff/pull/10729
synced_at: 2026-01-12T15:55:33Z
```

# Accept non-aliased (but correct) import in unconventional-import-alias

---

_@charliermarsh_

## Summary

Given:

```toml
[tool.ruff.lint.flake8-import-conventions.aliases]
"django.conf.settings" = "settings"
```

We should accept `from django.conf import settings`.

Closes https://github.com/astral-sh/ruff/issues/10599.


---

_Label `bug` added by @charliermarsh on 2024-04-02 03:34_

---

_@charliermarsh reviewed on 2024-04-02 03:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_import_conventions/rules/unconventional_import_alias.rs`:69 on 2024-04-02 03:34_

(This is the fix.)

---

_Comment by @github-actions[bot] on 2024-04-02 03:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-04-02 03:47_

---

_Closed by @charliermarsh on 2024-04-02 03:47_

---

_Branch deleted on 2024-04-02 03:47_

---
