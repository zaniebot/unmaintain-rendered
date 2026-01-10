```yaml
number: 21100
title: "[ty] Make auto-import skip symbols in current module"
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
head: ag/skip-self-completions
created_at: 2025-10-27T18:44:26Z
updated_at: 2025-10-29T13:13:51Z
url: https://github.com/astral-sh/ruff/pull/21100
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Make auto-import skip symbols in current module

---

_Pull request opened by @BurntSushi on 2025-10-27 18:44_

This adds a small check to ensure symbols from auto-import that are
defined in the current module aren't returned. In particular, these
symbols wind up being redundant and are just overall quite confusing.

It's best to review this commit-by-commit. In particular, the bulk of
this PR is a refactoring of the completion tests to make them a little
more declarative in more cases.

Fixes [#1445](https://github.com/astral-sh/ty/issues/1445)


---

_Review requested from @carljm by @BurntSushi on 2025-10-27 18:44_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-27 18:44_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-10-27 18:44_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-27 18:44_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-27 18:44_

---

_Label `bug` added by @BurntSushi on 2025-10-27 18:44_

---

_Label `server` added by @BurntSushi on 2025-10-27 18:44_

---

_Label `ty` added by @BurntSushi on 2025-10-27 18:44_

---

_Review request for @dcreager removed by @BurntSushi on 2025-10-27 18:45_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-27 18:45_

---

_Comment by @github-actions[bot] on 2025-10-27 18:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-27 18:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:4099 on 2025-10-28 17:58_

Nit: You'll probably role your eyes given that this is a test. Please feel free to disregard it ;)

Should we use `original.drain` when building `filtered` and a) only store the filtered out suggestions in `CompletionTest` (instead of `original`) or b) only store a bool saying if any suggestions were discarded.



---

_Review comment by @MichaReiser on `crates/ty_completion_eval/truth/auto-import-skips-current-module/main.py`:19 on 2025-10-28 18:03_

To make sure I understand the test case. This used to suggest `Kadabra` twice, once as local symbol and once as import?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:335 on 2025-10-28 18:06_

I misread this as a `if let Some()` and was very confused why skipping files without a module would fix this bug. Maybe rewrite to `if symbol.module.file(db) == Some(file)` to disambiguate?

---

_@MichaReiser approved on 2025-10-28 18:06_

---

_@BurntSushi reviewed on 2025-10-29 12:43_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:4099 on 2025-10-29 12:43_

Yeah I think since this is a test, I don't mind it. I was mostly just trying to write the simplest thing.

---

_@BurntSushi reviewed on 2025-10-29 12:46_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/auto-import-skips-current-module/main.py`:19 on 2025-10-29 12:46_

That's right. I added this comment:

```
Kadabra = 1

# This is meant to reflect that auto-import
# does *not* include completions for `Kadabra`.
# That is, before a bug was fixed, completions
# would offer two variants for `Kadabra`: one
# for the current module (correct) and another
# from auto-import that would insert
# `from main import Kadabra` into this module
# (incorrect).
#
# Since the incorrect one wasn't ranked above
# the correct one, this task unfortunately
# doesn't change the evaluation results. But
# I've added it anyway in case it does in the
# future (or if we change our evaluation metric
# to something that incorporates suggestions
# after the correct one).
Kada<CURSOR: Kadabra>
```

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:335 on 2025-10-29 12:47_

Hah yeah, that's fair.

---

_@BurntSushi reviewed on 2025-10-29 12:47_

---

_Merged by @BurntSushi on 2025-10-29 13:13_

---

_Closed by @BurntSushi on 2025-10-29 13:13_

---

_Branch deleted on 2025-10-29 13:13_

---
