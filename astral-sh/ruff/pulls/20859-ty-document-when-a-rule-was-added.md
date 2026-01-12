```yaml
number: 20859
title: "[ty] Document when a rule was added"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/document-when-rule-was-added
created_at: 2025-10-14T10:04:46Z
updated_at: 2025-10-14T12:33:50Z
url: https://github.com/astral-sh/ruff/pull/20859
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Document when a rule was added

---

_@MichaReiser_

## Summary

Replace the placeholder `1.0.0` version in the lint definitions with the version when they were first added to ty. 

Update the `rules.md` generation script to include the version in the documentation.

Closes https://github.com/astral-sh/ty/issues/828


---

_Label `documentation` added by @MichaReiser on 2025-10-14 10:04_

---

_Label `ty` added by @MichaReiser on 2025-10-14 10:04_

---

_Label `documentation` added by @MichaReiser on 2025-10-14 10:04_

---

_Label `ty` added by @MichaReiser on 2025-10-14 10:04_

---

_Comment by @github-actions[bot] on 2025-10-14 10:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 10:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-10-14 10:26_

---

_Review requested from @carljm by @MichaReiser on 2025-10-14 10:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-10-14 10:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-14 10:26_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-14 10:26_

---

_@sharkdp approved on 2025-10-14 12:04_

Thank you!

As for the actual versions that we put in, I think I would probably prefer them to point to our first stable release, once that's out?

Reading something like "introduced in 0.0.1-alpha.17" in the documentation for a core ty rule might look more confusing than helpful?

---

_Comment by @MichaReiser on 2025-10-14 12:33_

> As for the actual versions that we put in, I think I would probably prefer them to point to our first stable release, once that's out?

Yes, I think we can consider doing this once we have a stable release (or we update all versions to 0.0.1 with the beta released). 

---

_Merged by @MichaReiser on 2025-10-14 12:33_

---

_Closed by @MichaReiser on 2025-10-14 12:33_

---

_Branch deleted on 2025-10-14 12:33_

---
