```yaml
number: 18537
title: "[ty] Split CLI tests into multiple files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/extract-test-helpers
created_at: 2025-06-07T16:34:05Z
updated_at: 2025-06-07T16:43:44Z
url: https://github.com/astral-sh/ruff/pull/18537
synced_at: 2026-01-12T15:56:21Z
```

# [ty] Split CLI tests into multiple files

---

_@MichaReiser_

## Summary

The `cli.rs` file has become huge. Let's group the tests and move them into more specific test files.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-06-07 16:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-07 16:34_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-07 16:34_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-07 16:34_

---

_Label `testing` added by @MichaReiser on 2025-06-07 16:34_

---

_Label `ty` added by @MichaReiser on 2025-06-07 16:34_

---

_@AlexWaygood reviewed on 2025-06-07 16:36_

---

_Review comment by @AlexWaygood on `.claude/settings.local.json`:1 on 2025-06-07 16:36_

we should either add this to the repo's gitignore file or you should add it to your user gitignore file locally ;)

---

_Comment by @github-actions[bot] on 2025-06-07 16:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-06-07 16:40_

---

_Merged by @MichaReiser on 2025-06-07 16:43_

---

_Closed by @MichaReiser on 2025-06-07 16:43_

---

_Branch deleted on 2025-06-07 16:43_

---
