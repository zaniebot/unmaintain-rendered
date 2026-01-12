```yaml
number: 20947
title: Only add the actual schema in schemastore PRs
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - release
assignees: []
merged: true
base: main
head: david/schemastore
created_at: 2025-10-17T19:04:28Z
updated_at: 2025-10-17T19:15:18Z
url: https://github.com/astral-sh/ruff/pull/20947
synced_at: 2026-01-12T15:57:12Z
```

# Only add the actual schema in schemastore PRs

---

_@sharkdp_

Same as https://github.com/astral-sh/ty/pull/1391:

> Last time I ran this script, due to what I assume was a `npm` version mismatch, the `package-lock.json` file was updated while running `npm install` in the `schemastore`. Due to the use of `git commit -a`, it was accidentally included in the commit for the semi-automated schemastore PR. The solution here is to only add the actual file that we want to commit.

---

_Label `internal` added by @sharkdp on 2025-10-17 19:04_

---

_Label `release` added by @sharkdp on 2025-10-17 19:04_

---

_@AlexWaygood approved on 2025-10-17 19:12_

---

_Merged by @sharkdp on 2025-10-17 19:14_

---

_Closed by @sharkdp on 2025-10-17 19:14_

---

_Branch deleted on 2025-10-17 19:14_

---

_Comment by @github-actions[bot] on 2025-10-17 19:15_

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
