```yaml
number: 19870
title: "[ty] Fix tool name is None when no ty path is given in ty_benchmark"
type: pull_request
state: merged
author: nguu0123
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2025-08-11T21:17:49Z
updated_at: 2025-08-11T21:33:13Z
url: https://github.com/astral-sh/ruff/pull/19870
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix tool name is None when no ty path is given in ty_benchmark

---

_Pull request opened by @nguu0123 on 2025-08-11 21:17_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
When running the ty_benchmark, I found out that the Ty Tool name is None when no ty_path is given as str(None)='None'
<img width="1011" height="168" alt="image" src="https://github.com/user-attachments/assets/cf3e6d98-2329-48e9-b180-c72e4f01ccb6" />

## Test Plan
Minor fix, tested local
<img width="1105" height="218" alt="image" src="https://github.com/user-attachments/assets/173128c9-dcfa-49f1-a58d-1b39a6c6b53b" />

<!-- How was it tested? -->


---

_Review requested from @carljm by @nguu0123 on 2025-08-11 21:17_

---

_Review requested from @MichaReiser by @nguu0123 on 2025-08-11 21:17_

---

_Review requested from @AlexWaygood by @nguu0123 on 2025-08-11 21:17_

---

_Review requested from @sharkdp by @nguu0123 on 2025-08-11 21:17_

---

_Review requested from @dcreager by @nguu0123 on 2025-08-11 21:17_

---

_Renamed from "[ty] Fix tool name is None when no ty path is given ty_benchmark" to "[ty] Fix tool name is None when no ty path is given in ty_benchmark" by @nguu0123 on 2025-08-11 21:18_

---

_@carljm approved on 2025-08-11 21:23_

---

_Comment by @carljm on 2025-08-11 21:23_

Thank you!

---

_Merged by @carljm on 2025-08-11 21:26_

---

_Closed by @carljm on 2025-08-11 21:26_

---

_Comment by @github-actions[bot] on 2025-08-11 21:33_

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
