```yaml
number: 10007
title: "docs: Formatter compatibility warning for D207 and D300"
type: pull_request
state: merged
author: arkuhn
labels:
  - documentation
assignees: []
merged: true
base: main
head: akuhn/docs-formatter-redundancy
created_at: 2024-02-16T15:29:41Z
updated_at: 2024-12-28T23:44:56Z
url: https://github.com/astral-sh/ruff/pull/10007
synced_at: 2026-01-12T15:55:31Z
```

# docs: Formatter compatibility warning for D207 and D300

---

_@arkuhn_

- Update docs to mention formatter compatibility interactions for under-indentation (D207) and triple-single-quotes (D300)
- Changes verified locally with mkdocs
- Closes: https://github.com/astral-sh/ruff/issues/9675



---

_Comment by @github-actions[bot] on 2024-02-16 15:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @charliermarsh on 2024-02-17 12:36_

---

_Comment by @charliermarsh on 2024-02-17 12:36_

Thank you!

---

_Merged by @charliermarsh on 2024-02-17 12:37_

---

_Closed by @charliermarsh on 2024-02-17 12:37_

---

_Comment by @Avasam on 2024-12-28 23:44_

Are you sure D300 applies? The playground even with `preview=true` does not format those docstring back to triple-quoting: https://play.ruff.rs/c41e8485-df53-4dbd-a738-274ffc71c6fe?secondary=Format

---
