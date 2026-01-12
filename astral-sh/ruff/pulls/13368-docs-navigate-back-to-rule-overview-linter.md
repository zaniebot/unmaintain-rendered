```yaml
number: 13368
title: "DOCS: navigate back to rule overview linter"
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-navigate-linter
created_at: 2024-09-16T11:15:27Z
updated_at: 2024-09-16T16:42:58Z
url: https://github.com/astral-sh/ruff/pull/13368
synced_at: 2026-01-12T15:55:44Z
```

# DOCS: navigate back to rule overview linter

---

_@sbrugman_

Implementation for one of the suggestions in https://github.com/astral-sh/ruff/issues/13367

## Summary

Link back to the rule table section for a linter from the rule-specific pages.

## Test Plan

Tested locally with `mkdocs serve`:

<img src="https://github.com/user-attachments/assets/555f58ae-c364-4502-8568-35d57aba1bc4">

(`pycodestyle` is now a clickable link) 

---

_Comment by @github-actions[bot] on 2024-09-16 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_docs.rs`:37 on 2024-09-16 12:58_

What's the meaning of `dsv`? Maybe consider using a different name

---

_@MichaReiser approved on 2024-09-16 12:58_

Nice, this is great!

---

_Label `documentation` added by @MichaReiser on 2024-09-16 12:58_

---

_@sbrugman reviewed on 2024-09-16 16:15_

---

_Review comment by @sbrugman on `crates/ruff_dev/src/generate_docs.rs`:37 on 2024-09-16 16:15_

Renamed to `common_prefix`.

---

_Comment by @MichaReiser on 2024-09-16 16:20_

Thanks

---

_Merged by @MichaReiser on 2024-09-16 16:21_

---

_Closed by @MichaReiser on 2024-09-16 16:21_

---

_Branch deleted on 2024-09-16 16:42_

---
