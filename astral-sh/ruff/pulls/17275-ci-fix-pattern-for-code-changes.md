```yaml
number: 17275
title: "[ci] Fix pattern for code changes"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/fix-code-pattern
created_at: 2025-04-07T13:59:53Z
updated_at: 2025-04-07T14:11:20Z
url: https://github.com/astral-sh/ruff/pull/17275
synced_at: 2026-01-12T15:56:01Z
```

# [ci] Fix pattern for code changes

---

_@MichaReiser_


## Summary

`**/*` only matches files in a subdirectory whereas `**` matches any file at an arbitrary depth

> A trailing "/**" matches everything inside. For example, "abc/**" matches all files inside directory "abc", relative to the location of the .gitignore file, with infinite depth.

> A leading "**" followed by a slash means match in all directories. For example, "**/foo" matches file or directory "foo" anywhere, the same as pattern "foo". "**/foo/bar" matches file or directory "bar" anywhere that is directly under directory "foo".




---

_Label `ci` added by @MichaReiser on 2025-04-07 14:00_

---

_Merged by @MichaReiser on 2025-04-07 14:05_

---

_Closed by @MichaReiser on 2025-04-07 14:05_

---

_Branch deleted on 2025-04-07 14:05_

---

_Comment by @github-actions[bot] on 2025-04-07 14:11_

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
