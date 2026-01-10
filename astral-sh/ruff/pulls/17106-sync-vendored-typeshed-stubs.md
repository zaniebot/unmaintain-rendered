```yaml
number: 17106
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-04-01T00:33:12Z
updated_at: 2025-04-01T16:44:34Z
url: https://github.com/astral-sh/ruff/pull/17106
synced_at: 2026-01-10T19:40:37Z
```

# Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-04-01 00:33_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2025-04-01 00:33_

---

_Review requested from @carljm by @github-actions[bot] on 2025-04-01 00:33_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-04-01 00:33_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-04-01 00:33_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-04-01 00:33_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-04-01 00:33_

---

_Closed by @carljm on 2025-04-01 00:34_

---

_Reopened by @carljm on 2025-04-01 00:34_

---

_Comment by @github-actions[bot] on 2025-04-01 00:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-04-01 00:39_

Whoops, this brings in the new definition of `Any` as a class instead of just `object`, so we need to update to accommodate that first, or else it breaks our understanding of `Any` entirely.

---

_Comment by @carljm on 2025-04-01 01:30_

I rebased this on top of https://github.com/astral-sh/ruff/pull/17107 to fix our understanding of `Any`.

Now this PR has zero ecosystem impact.

---

_Comment by @AlexWaygood on 2025-04-01 16:44_

Primer hits are 0, CI is green. Mergin'

---

_Merged by @AlexWaygood on 2025-04-01 16:44_

---

_Closed by @AlexWaygood on 2025-04-01 16:44_

---

_Branch deleted on 2025-04-01 16:44_

---
