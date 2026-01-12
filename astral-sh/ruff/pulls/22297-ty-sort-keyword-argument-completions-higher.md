```yaml
number: 22297
title: "[ty] Sort keyword argument completions higher"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: fix-argument-completion-order
created_at: 2025-12-30T13:47:22Z
updated_at: 2026-01-06T20:19:37Z
url: https://github.com/astral-sh/ruff/pull/22297
synced_at: 2026-01-12T15:57:46Z
```

# [ty] Sort keyword argument completions higher

---

_@RasmusNygren_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2271

## Test Plan
Fix existing snapshot tests

---

_Review requested from @carljm by @RasmusNygren on 2025-12-30 13:47_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-30 13:47_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-30 13:47_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-30 13:47_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-30 13:47_

---

_@RasmusNygren reviewed on 2025-12-30 13:48_

---

_Review comment by @RasmusNygren on `crates/ty_completion_eval/truth/class-arg-completion/main.py`:1 on 2025-12-30 13:48_

We don't really capture the sort order of this very well in existing tests in `completion.rs::tests` so it probably makes sense to capture it here as an eval test

---

_Label `server` added by @AlexWaygood on 2025-12-30 13:48_

---

_Label `ty` added by @AlexWaygood on 2025-12-30 13:48_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-12-30 23:04_

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-05 12:35_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-05 12:35_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-05 12:35_

---

_@MichaReiser approved on 2026-01-06 10:50_

Thank you

---

_Merged by @MichaReiser on 2026-01-06 10:57_

---

_Closed by @MichaReiser on 2026-01-06 10:57_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:295 on 2026-01-06 19:48_

Yeah okay, I think I buy this. I'm slightly concerned about collapsing all context specific information down into a single bool, but I think this is fine for now.

---

_@BurntSushi reviewed on 2026-01-06 19:48_

Nice! Thank you!

---

_@RasmusNygren reviewed on 2026-01-06 20:13_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:295 on 2026-01-06 20:13_

Yeah I agree, I mainly did it like that because clippy started complaining about too many bools in that struct when I added another one and I didn't really feel the need to consider a bigger change at that point when I thought this seemed fine enough for now :) (and it does seem like a reasonable lint to obey and not just slap an ignore on)

---

_@BurntSushi reviewed on 2026-01-06 20:15_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:295 on 2026-01-06 20:15_

Hah. I will happily squash that lint for structs personally. (Too many bool parameters to a function is another story though.)

---

_@RasmusNygren reviewed on 2026-01-06 20:19_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:295 on 2026-01-06 20:19_

Maybe that's what I should have done instead then, for next time :)

---
