```yaml
number: 19334
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-07-14T16:17:32Z
updated_at: 2025-07-14T16:34:10Z
url: https://github.com/astral-sh/ruff/pull/19334
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-07-14 16:17_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-07-14 16:17_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-07-14 16:17_

---

_Label `ty` added by @github-actions[bot] on 2025-07-14 16:17_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-07-14 16:17_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-07-14 16:17_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-07-14 16:17_

---

_Comment by @AlexWaygood on 2025-07-14 16:24_

For future readers: the huge diff here is because we now codemod in the docstrings at typeshed-sync time, following https://github.com/astral-sh/ruff/pull/19311

---

_Comment by @github-actions[bot] on 2025-07-14 16:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~63MB
+     memo fields = ~66MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~152MB
+     memo fields = ~159MB

```
</details>


---

_@MichaReiser approved on 2025-07-14 16:28_

---

_Merged by @AlexWaygood on 2025-07-14 16:34_

---

_Closed by @AlexWaygood on 2025-07-14 16:34_

---

_Branch deleted on 2025-07-14 16:34_

---
