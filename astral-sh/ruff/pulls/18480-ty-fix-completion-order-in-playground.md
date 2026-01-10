```yaml
number: 18480
title: "[ty] Fix completion order in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/fix-completion-order
created_at: 2025-06-05T16:27:33Z
updated_at: 2025-06-05T16:55:56Z
url: https://github.com/astral-sh/ruff/pull/18480
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Fix completion order in playground

---

_Pull request opened by @MichaReiser on 2025-06-05 16:27_

## Summary

By default, Monaco sorts the completion items by the label but we want to preserve the order
returned by the `completions` functions. Ideally, we could just disable sorting but I couldn't find a way to do this.

This PR sets the `sortText` which monaco uses for sorting. It's a bit awkward because it's a string (which gets compared in alphabetic ordering)
But the basic idea is that we use the padded index as sort key.

## Test Plan

https://github.com/user-attachments/assets/acc589e6-bffe-4033-9de6-714dc1bd3e33




---

_Label `playground` added by @MichaReiser on 2025-06-05 16:27_

---

_Label `ty` added by @MichaReiser on 2025-06-05 16:27_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-06-05 16:27_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:498 on 2025-06-05 16:27_

That was a copy paste error of mine

---

_@MichaReiser reviewed on 2025-06-05 16:27_

---

_@BurntSushi approved on 2025-06-05 16:48_

Love it.

---

_Merged by @MichaReiser on 2025-06-05 16:55_

---

_Closed by @MichaReiser on 2025-06-05 16:55_

---

_Branch deleted on 2025-06-05 16:55_

---
