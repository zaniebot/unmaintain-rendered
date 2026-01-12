```yaml
number: 18322
title: "[ty] Move `respect-ignore-files` under `src` section"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/respect-ignore-files-src
created_at: 2025-05-26T15:17:55Z
updated_at: 2025-05-26T17:45:49Z
url: https://github.com/astral-sh/ruff/pull/18322
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Move `respect-ignore-files` under `src` section

---

_@MichaReiser_

## Summary

The option directly influences what files are included in the project. I, therefore, think it better fits into the `src` section. 

The main reason for having it under root was that other `src` level options are allowed at the project level whereas `respect-ignore-files` can only be
set at the workspace level. While it's nice to express this in the schema, I think it's also just fine to warn if we find such usage by emitting a diagnostic.

Closes https://github.com/astral-sh/ty/issues/503

## Test Plan

Updated cli tests


---

_Review requested from @carljm by @MichaReiser on 2025-05-26 15:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-26 15:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-26 15:17_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-26 15:17_

---

_Label `configuration` added by @MichaReiser on 2025-05-26 15:18_

---

_Label `ty` added by @MichaReiser on 2025-05-26 15:18_

---

_Comment by @github-actions[bot] on 2025-05-26 15:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp approved on 2025-05-26 17:44_

Thank you!

---

_Merged by @MichaReiser on 2025-05-26 17:45_

---

_Closed by @MichaReiser on 2025-05-26 17:45_

---

_Branch deleted on 2025-05-26 17:45_

---
