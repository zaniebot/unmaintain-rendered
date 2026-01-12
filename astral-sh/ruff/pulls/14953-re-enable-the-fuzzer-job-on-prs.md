```yaml
number: 14953
title: Re-enable the fuzzer job on PRs
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/renable-fuzz-on-pr
created_at: 2024-12-13T08:59:51Z
updated_at: 2024-12-13T09:12:08Z
url: https://github.com/astral-sh/ruff/pull/14953
synced_at: 2026-01-12T15:55:49Z
```

# Re-enable the fuzzer job on PRs

---

_@MichaReiser_

## Summary
This reverts https://github.com/astral-sh/ruff/pull/14478

I now broke main twice because I wasn't aware that the API was used by the fuzzer.

## Test Plan



---

_Label `ci` added by @MichaReiser on 2024-12-13 08:59_

---

_Merged by @MichaReiser on 2024-12-13 09:07_

---

_Closed by @MichaReiser on 2024-12-13 09:07_

---

_Branch deleted on 2024-12-13 09:07_

---

_Comment by @github-actions[bot] on 2024-12-13 09:12_

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
