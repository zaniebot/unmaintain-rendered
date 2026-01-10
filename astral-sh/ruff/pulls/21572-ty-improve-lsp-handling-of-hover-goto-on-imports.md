```yaml
number: 21572
title: "[ty] Improve lsp handling of hover/goto on imports"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/modty
created_at: 2025-11-21T19:58:56Z
updated_at: 2025-11-22T16:06:18Z
url: https://github.com/astral-sh/ruff/pull/21572
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve lsp handling of hover/goto on imports

---

_Pull request opened by @Gankra on 2025-11-21 19:58_

* Fixes https://github.com/astral-sh/ty/issues/1011
* Also fixes the fact that we didn't handle `.x` properly *at all* in hover/goto

It turns out all of our import handling completely ignored the `level` (number of relative `.`'s) in a `from ..x.y import z` statement. It was nice seeing how much my understanding of `ty` has improved -- previously this would have all been opaque to me but now it was just, completely glaring and blatant. 

Fixing this required refactoring all the import code to take the importing file into consideration. I ended up refactoring a bunch of code to pass around/require `SemanticModel` more, as it's the natural API for resolving this kind of import (it actually had an API for this that was just... dead code, whoops!).

---

_Review requested from @carljm by @Gankra on 2025-11-21 19:58_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-21 19:58_

---

_Label `server` added by @Gankra on 2025-11-21 19:58_

---

_Review requested from @sharkdp by @Gankra on 2025-11-21 19:58_

---

_Review requested from @dcreager by @Gankra on 2025-11-21 19:58_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-21 19:58_

---

_Label `ty` added by @Gankra on 2025-11-21 19:58_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 20:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 20:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:871 on 2025-11-22 14:34_

Sort of funny that `SemanticModel` survived to this day. I introduced it in one of the very earliest prototypes of a type aware linter back when ty was barely able to infer ints. I'm not sure it's the right API but good to see that it's not complete rubbish and is useful in cases like this :)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:942 on 2025-11-22 14:35_

I wonder if it makes sense to change this to `node.is_stmt`. I don't think we ever want to go beyond the closest statement (my, now proven incorrect, assumption was that `expression` is a sufficient boundary but turns out it isn't)

---

_@MichaReiser approved on 2025-11-22 14:38_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-22 15:09_

---

_@Gankra reviewed on 2025-11-22 16:05_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:871 on 2025-11-22 16:05_

Yeah between this PR and the string annotations one I am Fully embracing it as an actually useful abstraction!

---

_@Gankra reviewed on 2025-11-22 16:06_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:942 on 2025-11-22 16:06_

Hmm I'll think about it

---

_Merged by @Gankra on 2025-11-22 16:06_

---

_Closed by @Gankra on 2025-11-22 16:06_

---

_Branch deleted on 2025-11-22 16:06_

---
