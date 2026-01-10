```yaml
number: 17943
title: Update ty metadata
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/ty-metadata
created_at: 2025-05-08T11:11:04Z
updated_at: 2025-05-08T11:24:33Z
url: https://github.com/astral-sh/ruff/pull/17943
synced_at: 2026-01-10T18:57:03Z
```

# Update ty metadata

---

_Pull request opened by @MichaReiser on 2025-05-08 11:11_

I noticed that the Homepage link on pypi is incorrect. It seems that maturin always picks the link from the crate metadata. This PR updatesu pdates the `Documentation` and `Homepage` metadata fields for the `ty` crate and points them at the ty repository. 

## Test plan

I ran `uv build` in the ty repository (after updating the ruff submodule to this commit) and verified that the `PKG-INFO` only contains links pointing to ty.



---

_Comment by @github-actions[bot] on 2025-05-08 11:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @MichaReiser on 2025-05-08 11:18_

---

_Marked ready for review by @MichaReiser on 2025-05-08 11:18_

---

_Review requested from @carljm by @MichaReiser on 2025-05-08 11:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-08 11:18_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-08 11:18_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-08 11:18_

---

_Merged by @MichaReiser on 2025-05-08 11:24_

---

_Closed by @MichaReiser on 2025-05-08 11:24_

---

_Branch deleted on 2025-05-08 11:24_

---
