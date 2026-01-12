```yaml
number: 22173
title: "[ty] Fix module resolution on network drives"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - windows
  - ty
assignees: []
merged: true
base: main
head: micha/fix-case-sensitive-fast-check-unc-paths
created_at: 2025-12-24T14:30:23Z
updated_at: 2025-12-24T15:36:20Z
url: https://github.com/astral-sh/ruff/pull/22173
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Fix module resolution on network drives

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2062

This is a very silly bug (I'm allowed to say this because I wrote the code :)). The comment on `simplify_ignore_verbatim` explicitly states that the simplified path should only be used for comparision. And what did I do??? I use it as an argument to `canonicalize` which immediately fails because the simplified path isn't a valid path. 

The fix is trivial. Call canonicalize on the not-simplified path and only use the simplified path when comparing the two paths.

## Test plan

I verified that the imports in https://github.com/astral-sh/ty/issues/2062 now resovle successfully. 

I'm not sure how to write a test for a network drive...

---

_Label `bug` added by @MichaReiser on 2025-12-24 14:30_

---

_Label `windows` added by @MichaReiser on 2025-12-24 14:30_

---

_Label `ty` added by @MichaReiser on 2025-12-24 14:30_

---

_Label `bug` added by @MichaReiser on 2025-12-24 14:30_

---

_Label `windows` added by @MichaReiser on 2025-12-24 14:30_

---

_Label `ty` added by @MichaReiser on 2025-12-24 14:30_

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 14:37_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @MichaReiser on 2025-12-24 14:37_

---

_Review requested from @carljm by @MichaReiser on 2025-12-24 14:37_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-24 14:37_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-24 14:37_

---

_@charliermarsh approved on 2025-12-24 14:38_

---

_Merged by @MichaReiser on 2025-12-24 15:36_

---

_Closed by @MichaReiser on 2025-12-24 15:36_

---

_Branch deleted on 2025-12-24 15:36_

---
