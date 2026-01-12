```yaml
number: 17158
title: "ruff_db: simplify lifetimes on `DiagnosticDisplay`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/simplify-lifetimes
created_at: 2025-04-02T16:30:19Z
updated_at: 2025-04-02T16:47:03Z
url: https://github.com/astral-sh/ruff/pull/17158
synced_at: 2026-01-12T15:56:00Z
```

# ruff_db: simplify lifetimes on `DiagnosticDisplay`

---

_@BurntSushi_

I initially split the lifetime out into three distinct lifetimes on
near-instinct because I moved the struct into the public API. But
because they are all shared borrows, and because there are no other APIs
on `DisplayDiagnostic` to access individual fields (and probably never
will be), it's probably fine to just specify one lifetime. Because of
subtyping, the one lifetime will be the shorter of the three.

There's also the point that `ruff_db` isn't _really_ a public API, since
it isn't a library that others depend on. So my instinct is probably a
bit off there.


---

_Review requested from @carljm by @BurntSushi on 2025-04-02 16:30_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-02 16:30_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-02 16:30_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-02 16:30_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-02 16:30_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-02 16:30_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-02 16:30_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-02 16:30_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-02 16:30_

---

_Label `red-knot` added by @MichaReiser on 2025-04-02 16:44_

---

_@MichaReiser approved on 2025-04-02 16:45_

---

_Merged by @BurntSushi on 2025-04-02 16:47_

---

_Closed by @BurntSushi on 2025-04-02 16:47_

---

_Branch deleted on 2025-04-02 16:47_

---
