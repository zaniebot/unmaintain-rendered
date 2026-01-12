```yaml
number: 17888
title: "Display `ty version` for `ty --version` and `ty -V`"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/version-iii
created_at: 2025-05-06T12:36:26Z
updated_at: 2025-05-06T13:06:43Z
url: https://github.com/astral-sh/ruff/pull/17888
synced_at: 2026-01-12T15:56:07Z
```

# Display `ty version` for `ty --version` and `ty -V`

---

_@zanieb_

e.g.,

```
❯ uv run -q -- ty -V
ty 0.0.0-alpha.4 (08881edba 2025-05-05)
❯ uv run -q -- ty --version
ty 0.0.0-alpha.4 (08881edba 2025-05-05)
```

Previously, this just displayed `ty 0.0.0` because it didn't use our custom version implementation. We no longer have a short version — matching the interface in uv. We could add a variant for it, if it seems important to people. However, I think we found it more confusing than not over there and didn't get any complaints about the change.

Closes https://github.com/astral-sh/ty/issues/54

---

_Review requested from @carljm by @zanieb on 2025-05-06 12:36_

---

_Review requested from @MichaReiser by @zanieb on 2025-05-06 12:36_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-06 12:36_

---

_Review requested from @sharkdp by @zanieb on 2025-05-06 12:36_

---

_Review requested from @dcreager by @zanieb on 2025-05-06 12:36_

---

_Label `ty` added by @zanieb on 2025-05-06 12:36_

---

_Comment by @github-actions[bot] on 2025-05-06 12:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-05-06 12:46_

---

_Merged by @zanieb on 2025-05-06 13:06_

---

_Closed by @zanieb on 2025-05-06 13:06_

---

_Branch deleted on 2025-05-06 13:06_

---
