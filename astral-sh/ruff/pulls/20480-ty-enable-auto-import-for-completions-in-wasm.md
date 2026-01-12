```yaml
number: 20480
title: "[ty] Enable auto-import for completions in WASM builds by default"
type: pull_request
state: merged
author: BurntSushi
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: ag/enable-auto-import-by-default
created_at: 2025-09-19T12:08:10Z
updated_at: 2025-09-25T14:19:52Z
url: https://github.com/astral-sh/ruff/pull/20480
synced_at: 2026-01-12T15:57:02Z
```

# [ty] Enable auto-import for completions in WASM builds by default

---

_@BurntSushi_

Now that imports are actually inserted, this should give us some
valuable dog-fooding experience.

Note that we don't currently do any ranking on completions, so until
that is improved, even in-scope completions could suffer. With that
said, this shouldn't have any impact at all in several scenarios (like
completions for attributes on objects).


---

_Review requested from @carljm by @BurntSushi on 2025-09-19 12:08_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-19 12:08_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-19 12:08_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-19 12:08_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-19 12:08_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-19 12:08_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-09-19 12:08_

---

_Label `playground` added by @MichaReiser on 2025-09-19 12:09_

---

_@MichaReiser approved on 2025-09-19 12:09_

---

_Comment by @github-actions[bot] on 2025-09-19 12:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Merged by @BurntSushi on 2025-09-19 12:11_

---

_Closed by @BurntSushi on 2025-09-19 12:11_

---

_Branch deleted on 2025-09-19 12:12_

---

_Comment by @github-actions[bot] on 2025-09-19 12:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @ntBre on 2025-09-25 14:19_

---
