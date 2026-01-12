```yaml
number: 15883
title: "nit: docs for ignore & select"
type: pull_request
state: merged
author: anordin95
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-02-02T19:31:57Z
updated_at: 2025-02-04T09:05:42Z
url: https://github.com/astral-sh/ruff/pull/15883
synced_at: 2026-01-12T15:55:53Z
```

# nit: docs for ignore & select

---

_@anordin95_

## Clarity for ignore & select when the same prefix is in both

I initially read this and was curious what would happen if the same prefix appeared in both ignore & select. 


---

_Comment by @MichaReiser on 2025-02-02 19:38_

Thanks. The documentation is auto-generated. It is taken from the comment in https://github.com/astral-sh/ruff/blob/6090408f65ba1f5f4c8fd9e92174d5a89370860f/crates/ruff_workspace/src/options.rs#L653-L668

---

_Label `documentation` added by @MichaReiser on 2025-02-02 19:38_

---

_Comment by @anordin95 on 2025-02-02 20:09_

Ah, gotcha, thanks! Updated accordingly.

---

_Comment by @MichaReiser on 2025-02-03 14:14_

Now you have to run `cargo dev generate-all` to generate the JSON schema. It's around multiple corners, sorry for that

---

_Comment by @anordin95 on 2025-02-03 18:53_

No worries! Will do :)

PS: ah I noticed something that looked like a snapshot related test was failing. Updated that too.

---

_Comment by @github-actions[bot] on 2025-02-03 19:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-02-04 09:05_

---

_Closed by @MichaReiser on 2025-02-04 09:05_

---
