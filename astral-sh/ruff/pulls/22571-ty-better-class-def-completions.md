```yaml
number: 22571
title: "[ty] Better class def completions"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: better-class-def-completions
created_at: 2026-01-14T14:28:57Z
updated_at: 2026-01-15T18:26:35Z
url: https://github.com/astral-sh/ruff/pull/22571
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Better class def completions

---

_@RasmusNygren_

## Summary

Prefer completions of type `ClassLiteral` inside class-def context.

Fixes https://github.com/astral-sh/ty/issues/1776

## Test Plan
New completion-eval test that has incorrect ranking on main but correct after this patch.


---

_Review requested from @carljm by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @MichaReiser by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @sharkdp by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @dcreager by @RasmusNygren on 2026-01-14 14:28_

---

_Review requested from @Gankra by @RasmusNygren on 2026-01-14 14:28_

---

_Label `server` added by @AlexWaygood on 2026-01-14 14:29_

---

_Label `ty` added by @AlexWaygood on 2026-01-14 14:29_

---

_@RasmusNygren reviewed on 2026-01-14 14:47_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:853 on 2026-01-14 14:47_

This leads to us calculating whether or not we're in a class def twice, here and in `add_argument_completions`. I think this is fine for now but we probably want to do some refactors here and store more information on the context such that we only run the detection once and can re-use that both to add completions and for ranking. Perhaps the `NonImportContext` can store another enum and track if we're in a class-def/raising_exception etc.

Have you already thought about how to best scale this @BurntSushi ?

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-14 20:47_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-14 20:47_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-14 20:47_

---

_Review requested from @BurntSushi by @MichaReiser on 2026-01-14 20:47_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:853 on 2026-01-15 13:29_

I haven't thought too much about this, no. But what you say sounds plausible to me. Possibly the challenge here is making sure we don't try to pre-compute too much stuff, particularly if we end up not needing it.

But yeah I agree that this is fine for now. Especially given that `covering_node` is only computed once now. :-)

---

_@BurntSushi approved on 2026-01-15 13:30_

Beautiful. This looks great as-is, thank you!!!

---

_Merged by @BurntSushi on 2026-01-15 13:31_

---

_Closed by @BurntSushi on 2026-01-15 13:31_

---

_Branch deleted on 2026-01-15 18:26_

---
