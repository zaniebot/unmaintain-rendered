```yaml
number: 22293
title: "[ty] Add skip_dunders option to CompletionTestBuilder"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: add-skip-dunders-to-completions-test
created_at: 2025-12-29T23:04:54Z
updated_at: 2026-01-15T18:26:47Z
url: https://github.com/astral-sh/ruff/pull/22293
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Add skip_dunders option to CompletionTestBuilder

---

_@RasmusNygren_

This is useful to remove noise when asserting
snapshots on method or attribute completions.

## Summary

Most tests that checks completions on attributes and methods are written on the form `builder.build().contains("x").not_contains("y")` instead of asserting snapshots. These tests would be more robust if they were asserting exact snapshots (and IMO, in most cases also easier to read). This is however not really sustainable to do as long as we suggest dunders, as the snapshots become very large.

This adds a helper to skip dunder suggestions in the completion test builder.

## Test Plan
This only changes test ergonomics and is not enabled by default. A single test was changed to use this helper to avoid hitting dead code lints.


---

_Review requested from @carljm by @RasmusNygren on 2025-12-29 23:04_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-29 23:04_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-29 23:04_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-29 23:04_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-29 23:04_

---

_@RasmusNygren reviewed on 2025-12-29 23:08_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:4030 on 2025-12-29 23:08_

Essentially an arbitrary test that I changed (that can make use of this) to not add dead code.

There a lot of other tests that could make nice use of this that I intend to change if this lands, but I didn't want to overdo it ahead of time in case this functionality is not desirable.

---

_Review request for @carljm removed by @carljm on 2025-12-29 23:23_

---

_Label `testing` added by @MichaReiser on 2025-12-30 07:59_

---

_Label `ty` added by @MichaReiser on 2025-12-30 07:59_

---

_Comment by @MichaReiser on 2025-12-30 08:06_

Thank you. This makes sense to me.

---

_Merged by @MichaReiser on 2025-12-30 08:10_

---

_Closed by @MichaReiser on 2025-12-30 08:10_

---

_Branch deleted on 2026-01-15 18:26_

---
