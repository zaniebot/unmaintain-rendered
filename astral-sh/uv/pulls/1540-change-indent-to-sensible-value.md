```yaml
number: 1540
title: change indent to sensible value
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fix-indent
created_at: 2024-02-16T20:53:41Z
updated_at: 2024-02-16T21:11:27Z
url: https://github.com/astral-sh/uv/pull/1540
synced_at: 2026-01-12T16:04:39Z
```

# change indent to sensible value

---

_@BurntSushi_

_No description provided._

---

_@MichaReiser approved on 2024-02-16 20:54_

That's a very opinionated commit message ;)

---

_@zanieb approved on 2024-02-16 20:54_

🚲 

---

_Label `internal` added by @MichaReiser on 2024-02-16 20:54_

---

_Review comment by @AlexWaygood on `.editorconfig`:14 on 2024-02-16 20:55_

Or do [what we do at ruff](https://github.com/astral-sh/ruff/blob/20217e9bbda5434288d96454e90cb0846c1f330f/.editorconfig#L11-L14)?

```suggestion
indent_size = 2

[*.{rs,py,pyi}]
indent_size = 4
```

---

_@AlexWaygood approved on 2024-02-16 20:55_

---

_@BurntSushi reviewed on 2024-02-16 21:04_

---

_Review comment by @BurntSushi on `.editorconfig`:14 on 2024-02-16 21:04_

Fair enough. PR updated to just be a copy of what's in Ruff.

---

_Merged by @BurntSushi on 2024-02-16 21:11_

---

_Closed by @BurntSushi on 2024-02-16 21:11_

---

_Branch deleted on 2024-02-16 21:11_

---
