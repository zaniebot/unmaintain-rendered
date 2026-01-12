```yaml
number: 20315
title: "Allow the `if_not_else` Clippy lint"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/leave-my-negations-alone
created_at: 2025-09-09T12:17:32Z
updated_at: 2025-09-09T12:49:27Z
url: https://github.com/astral-sh/ruff/pull/20315
synced_at: 2026-01-12T15:56:59Z
```

# Allow the `if_not_else` Clippy lint

---

_@BurntSushi_

Specifically, the [`if_not_else`] lint will sometimes flag
code to change the order of `if` and `else` bodies if this
would allow a `!` to be removed. While perhaps tasteful in
some cases, there are many cases in my experience where this
bows to other competing concerns that impact readability.
(Such as the relative sizes of the `if` and `else` bodies,
or perhaps an ordering that just makes the code flow in a
more natural way.)

[`if_not_else`]: https://rust-lang.github.io/rust-clippy/master/index.html#/if_not_else


---

_Review requested from @carljm by @BurntSushi on 2025-09-09 12:17_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-09 12:17_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-09 12:17_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-09 12:17_

---

_Comment by @github-actions[bot] on 2025-09-09 12:19_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@dylwil3 approved on 2025-09-09 12:20_

---

_Comment by @github-actions[bot] on 2025-09-09 12:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-09-09 12:25_

---

_@zsol approved on 2025-09-09 12:30_

Approve harder

---

_Comment by @github-actions[bot] on 2025-09-09 12:32_

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

_Merged by @BurntSushi on 2025-09-09 12:49_

---

_Closed by @BurntSushi on 2025-09-09 12:49_

---

_Branch deleted on 2025-09-09 12:49_

---
