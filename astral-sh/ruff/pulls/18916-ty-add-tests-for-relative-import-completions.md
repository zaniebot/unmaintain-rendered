```yaml
number: 18916
title: "[ty] Add tests for relative import completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/test-relative-import-completions
created_at: 2025-06-24T14:04:42Z
updated_at: 2025-06-24T15:41:36Z
url: https://github.com/astral-sh/ruff/pull/18916
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Add tests for relative import completions

---

_@BurntSushi_

We first do a little refactoring here to make the test setup nicer.
Basically, we make it so a cursor test can have multiple files *and*
the caller can control which file contains the `<CURSOR>` marker.

Once that's done, we add a few tests confirming that we can recognize
relative `from ... import` statements and offer completions for them.

It is probably easiest to review this commit by commit.

Ref https://github.com/astral-sh/ruff/pull/18830#discussion_r2159670033


---

_Review requested from @carljm by @BurntSushi on 2025-06-24 14:04_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-24 14:04_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-24 14:04_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-24 14:04_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-24 14:04_

---

_Review request for @dcreager removed by @BurntSushi on 2025-06-24 14:04_

---

_Review request for @carljm removed by @BurntSushi on 2025-06-24 14:04_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-06-24 14:04_

---

_Label `internal` added by @BurntSushi on 2025-06-24 14:05_

---

_Label `ty` added by @BurntSushi on 2025-06-24 14:05_

---

_Comment by @github-actions[bot] on 2025-06-24 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_ide/src/lib.rs`:302 on 2025-06-24 15:14_

Should this error if `cursor` is already `Some` to avoid the case where a test has multiple (unintentional `CURSOR` positions)?

And/or could we do this already in `source` (so that the panic triggers immediately instead of when calling `build`)

---

_@MichaReiser approved on 2025-06-24 15:14_

Nice

---

_Review comment by @BurntSushi on `crates/ty_ide/src/lib.rs`:302 on 2025-06-24 15:16_

Yeah the line above has an `assert` for it.

I can add another assert to `source` as well. I agree that will give a nicer panic.

---

_@BurntSushi reviewed on 2025-06-24 15:16_

---

_@MichaReiser reviewed on 2025-06-24 15:17_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/lib.rs`:302 on 2025-06-24 15:17_

Lol, blind me :)

---

_@AlexWaygood reviewed on 2025-06-24 15:18_

Thank you!! This looks great from a quick skim. (I won't take a deeper look as Micha already reviewed 😄)

---

_Merged by @BurntSushi on 2025-06-24 15:41_

---

_Closed by @BurntSushi on 2025-06-24 15:41_

---

_Branch deleted on 2025-06-24 15:41_

---

_Label `server` added by @BurntSushi on 2025-06-24 15:41_

---
