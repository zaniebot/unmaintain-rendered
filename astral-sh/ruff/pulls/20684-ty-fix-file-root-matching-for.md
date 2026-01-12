```yaml
number: 20684
title: "[ty] Fix file root matching for `/`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/fix-1277
created_at: 2025-10-02T17:53:02Z
updated_at: 2025-10-03T12:18:05Z
url: https://github.com/astral-sh/ruff/pull/20684
synced_at: 2026-01-12T15:57:07Z
```

# [ty] Fix file root matching for `/`

---

_@BurntSushi_

Previously, we would always add `/{*filepath}` as our wildcard to match
descendant paths. But when the root is just `/` (as it can be in tests,
weird environments or in the ty playground), this causes a double `/`
and inhibits most descendant matches.

The regression test added in this commit fails without this fix.
Specifically, it panics because it can't find a file root for
`/project`.

This also enables the `log` feature on `tracing`, which means we get
better debug messages in the JavaScript console when running the
playground.

Fixes https://github.com/astral-sh/ty/issues/1277

---

_Review requested from @carljm by @BurntSushi on 2025-10-02 17:53_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-10-02 17:53_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-02 17:53_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-02 17:53_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-02 17:53_

---

_Review request for @dcreager removed by @BurntSushi on 2025-10-02 17:53_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-02 17:53_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-10-02 17:53_

---

_Label `bug` added by @BurntSushi on 2025-10-02 17:54_

---

_Label `server` added by @BurntSushi on 2025-10-02 17:54_

---

_Renamed from "ag/fix 1277" to "[ty] Fix file root matching for `/`" by @BurntSushi on 2025-10-02 17:54_

---

_Comment by @github-actions[bot] on 2025-10-02 17:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-02 17:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-02 18:06_

---

_Comment by @github-actions[bot] on 2025-10-02 18:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@ibraheemdev approved on 2025-10-02 19:03_

---

_Review comment by @MichaReiser on `Cargo.toml`:180 on 2025-10-02 19:43_

Can we only enable this in ty_wasm? 

It makes it a bit easier to reason about the change

---

_@MichaReiser approved on 2025-10-02 19:43_

---

_@BurntSushi reviewed on 2025-10-03 12:01_

---

_Review comment by @BurntSushi on `Cargo.toml`:180 on 2025-10-03 12:01_

I think it makes sense to enable this "globally," but I get wanting to do it narrowly. I've switched the PR to enabling `log` only for `ty_wasm`.

---

_Merged by @BurntSushi on 2025-10-03 12:18_

---

_Closed by @BurntSushi on 2025-10-03 12:18_

---

_Branch deleted on 2025-10-03 12:18_

---
