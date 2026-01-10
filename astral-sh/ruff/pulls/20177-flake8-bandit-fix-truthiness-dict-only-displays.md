```yaml
number: 20177
title: "[`flake8-bandit`] Fix truthiness: dict-only `**` displays not truthy for `shell` (`S602`, `S604`, `S609`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-19927
created_at: 2025-08-31T14:40:23Z
updated_at: 2025-09-10T21:45:07Z
url: https://github.com/astral-sh/ruff/pull/20177
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-bandit`] Fix truthiness: dict-only `**` displays not truthy for `shell` (`S602`, `S604`, `S609`)

---

_Pull request opened by @danparizher on 2025-08-31 14:40_

## Summary
Fixes #19927


---

_Comment by @github-actions[bot] on 2025-08-31 14:53_

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

_Comment by @dscorbett on 2025-08-31 18:20_

With this change, `{**{}}` is considered falsey; what about `{**{**{}}}`?

---

_Comment by @danparizher on 2025-08-31 19:08_

> With this change, `{**{}}` is considered falsey; what about `{**{**{}}}`?

Good call, I had nested dict displays as truthy. I just pushed a change to have them checked recursively.

---

_Label `bug` added by @ntBre on 2025-09-09 18:19_

---

_Label `rule` added by @ntBre on 2025-09-09 18:19_

---

_@ntBre reviewed on 2025-09-09 18:39_

---

_Review requested from @ntBre by @danparizher on 2025-09-09 20:48_

---

_Comment by @danparizher on 2025-09-09 20:49_

The CI failures seem to be unrelated to my changes, wondering if the one for `cargo fuzz build` was introduced elsewhere

---

_Comment by @ntBre on 2025-09-09 20:59_

Yeah no worries about the fuzz build. There was a breaking libcst release that we need to handle.

---

_@ntBre approved on 2025-09-10 20:50_

Thank you! I'll probably close and reopen to re-trigger CI, but this looks ready to go once it passes.

---

_Closed by @ntBre on 2025-09-10 20:50_

---

_Reopened by @ntBre on 2025-09-10 20:50_

---

_Merged by @ntBre on 2025-09-10 21:06_

---

_Closed by @ntBre on 2025-09-10 21:06_

---

_Branch deleted on 2025-09-10 21:45_

---
