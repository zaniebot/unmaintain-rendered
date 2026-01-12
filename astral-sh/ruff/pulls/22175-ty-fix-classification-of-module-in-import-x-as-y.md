```yaml
number: 22175
title: "[ty] Fix classification of module in `import x as y`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-import-as-classification
created_at: 2025-12-24T15:14:39Z
updated_at: 2025-12-25T23:32:37Z
url: https://github.com/astral-sh/ruff/pull/22175
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Fix classification of module in `import x as y`

---

_@MichaReiser_

## Summary

Previously, ty didn't classify the module if an import statement had an alias (e.g. `import pathlib as x`). 

This PR ensures that ty always creates the tokens for the imported module, regardless whether it has any alias, aligning it with pylance.


I also renamed removed the `test_` prefix from all tests

Fixes https://github.com/astral-sh/ty/issues/2149

## Test Plan

Added tests


---

_Review requested from @carljm by @MichaReiser on 2025-12-24 15:14_

---

_Label `bug` added by @MichaReiser on 2025-12-24 15:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-24 15:14_

---

_Label `server` added by @MichaReiser on 2025-12-24 15:14_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-24 15:14_

---

_Label `ty` added by @MichaReiser on 2025-12-24 15:14_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-24 15:14_

---

_@MichaReiser reviewed on 2025-12-24 15:15_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:233 on 2025-12-24 15:15_

VS Code logs a warning for empty ranges, saying that they're invalid. Empty tokens are also pretty useless because how do you style a token that has no representation in the source :)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-24 17:04_

---

_@charliermarsh approved on 2025-12-24 17:14_

---

_Merged by @MichaReiser on 2025-12-24 17:25_

---

_Closed by @MichaReiser on 2025-12-24 17:25_

---

_Branch deleted on 2025-12-24 17:25_

---

_@Pierce5080 reviewed on 2025-12-25 23:20_

![image](https://github.com/user-attachments/assets/1f37d2f5-9ce9-4ef4-9c37-30cdbfe0253c)

---

_@Pierce5080 reviewed on 2025-12-25 23:25_

[]()

---

_Comment by @Pierce5080 on 2025-12-25 23:26_

> ![image](https://github.com/user-attachments/assets/1f37d2f5-9ce9-4ef4-9c37-30cdbfe0253c)



---
