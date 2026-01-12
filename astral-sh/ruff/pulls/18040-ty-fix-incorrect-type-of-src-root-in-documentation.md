```yaml
number: 18040
title: "[ty] Fix incorrect type of `src.root` in documentation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/src-root-not-a-list
created_at: 2025-05-12T12:25:04Z
updated_at: 2025-05-12T12:29:17Z
url: https://github.com/astral-sh/ruff/pull/18040
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Fix incorrect type of `src.root` in documentation

---

_@MichaReiser_

## Summary

`src.root` is a single path. We do have plans to change the setting to a list (need to think about if we should allow empty lists) https://github.com/astral-sh/ty/issues/179 but we can change that later.

## Test Plan

See updated docs


---

_Review requested from @carljm by @MichaReiser on 2025-05-12 12:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-12 12:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-12 12:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-12 12:25_

---

_Label `documentation` added by @MichaReiser on 2025-05-12 12:25_

---

_Label `docstring` added by @MichaReiser on 2025-05-12 12:25_

---

_Label `ty` added by @MichaReiser on 2025-05-12 12:25_

---

_Label `docstring` removed by @MichaReiser on 2025-05-12 12:25_

---

_@AlexWaygood approved on 2025-05-12 12:26_

---

_@AlexWaygood reviewed on 2025-05-12 12:27_

---

_Review comment by @AlexWaygood on `crates/ty/docs/configuration.md`:167 on 2025-05-12 12:27_

Hmm, it's weird that we default to multiple values if users can't set it to multiple values

---

_Review comment by @MichaReiser on `crates/ty/docs/configuration.md`:167 on 2025-05-12 12:27_

Yeah, it is. Not sure how to fix.

---

_@MichaReiser reviewed on 2025-05-12 12:27_

---

_Merged by @MichaReiser on 2025-05-12 12:28_

---

_Closed by @MichaReiser on 2025-05-12 12:28_

---

_Branch deleted on 2025-05-12 12:28_

---

_Comment by @github-actions[bot] on 2025-05-12 12:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-05-12 12:29_

---

_Review comment by @AlexWaygood on `crates/ty/docs/configuration.md`:167 on 2025-05-12 12:29_

Maybe instead of referring to a default value, we could say something like

```suggestion
**If not set, two first-party roots are inferred by default: `"."` and `"./src"`**
```

?

---
