```yaml
number: 22465
title: "[ty] Rank top-level module symbols above most other symbols"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/rank-module-names-higher
created_at: 2026-01-08T20:09:08Z
updated_at: 2026-01-09T15:11:39Z
url: https://github.com/astral-sh/ruff/pull/22465
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Rank top-level module symbols above most other symbols

---

_Pull request opened by @BurntSushi on 2026-01-08 20:09_

This makes it so, e.g., `os<CURSOR>` will suggest the top-level stdlib
`os` module even if there is an `os` symbol elsewhere in your project.

The way this is done is somewhat overwrought, but it's done to avoid
suggesting top-level modules over other symbols already in scope.

Fixes https://github.com/astral-sh/ty/issues/1852


---

_Review requested from @carljm by @BurntSushi on 2026-01-08 20:09_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-08 20:09_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-08 20:09_

---

_Review requested from @sharkdp by @BurntSushi on 2026-01-08 20:09_

---

_Review requested from @dcreager by @BurntSushi on 2026-01-08 20:09_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-08 20:09_

---

_Review request for @dcreager removed by @BurntSushi on 2026-01-08 20:09_

---

_Review request for @carljm removed by @BurntSushi on 2026-01-08 20:09_

---

_Review request for @Gankra removed by @BurntSushi on 2026-01-08 20:09_

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-08 20:09_

---

_Label `server` added by @BurntSushi on 2026-01-08 20:10_

---

_Label `ty` added by @BurntSushi on 2026-01-08 20:10_

---

_@MichaReiser approved on 2026-01-09 08:23_

---

_Merged by @BurntSushi on 2026-01-09 15:11_

---

_Closed by @BurntSushi on 2026-01-09 15:11_

---

_Branch deleted on 2026-01-09 15:11_

---
