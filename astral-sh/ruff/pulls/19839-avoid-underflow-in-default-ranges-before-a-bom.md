```yaml
number: 19839
title: Avoid underflow in default ranges before a BOM
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/defuse-bom
created_at: 2025-08-08T21:29:54Z
updated_at: 2025-08-11T12:52:29Z
url: https://github.com/astral-sh/ruff/pull/19839
synced_at: 2026-01-10T17:52:17Z
```

# Avoid underflow in default ranges before a BOM

---

_Pull request opened by @ntBre on 2025-08-08 21:29_

Summary
--

This fixes a regression caused by the BOM handling in #19806. Most diagnostics already account for the BOM in their ranges, but those that use `TextRange::default` to mean the beginning of the file do not, causing an underflow in `RenderableAnnotation::new` when subtracting the BOM-shifted `snippet_start` from the annotation range.

I ran into this when trying to run benchmarks on CPython in preparation for caching work. The file `cpython/Lib/test/bad_coding2.py` was causing a crash because it had a default-range `I002` diagnostic, with a BOM.

https://github.com/astral-sh/ruff/blob/7cc3f1ebe9386e77e7009bc411fc6480d3851015/crates/ruff_linter/src/rules/isort/rules/add_required_imports.rs#L122-L126

The fix here is just to saturate to zero instead of panicking. I considered adding a `TextRange::saturating_sub` method, but I wasn't sure it was worth it for this one use. I'm happy to do that if preferred, though.

Saturating seemed easier than shifting the affected annotations over, but that could be another solution.

Test Plan
--

A new `ruff_db` test that reproduced the issue and manual testing against the CPython file mentioned above


---

_Label `internal` added by @ntBre on 2025-08-08 21:30_

---

_Marked ready for review by @ntBre on 2025-08-08 21:43_

---

_Review requested from @carljm by @ntBre on 2025-08-08 21:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-08 21:43_

---

_Review requested from @sharkdp by @ntBre on 2025-08-08 21:43_

---

_Review requested from @dcreager by @ntBre on 2025-08-08 21:43_

---

_Review request for @dcreager removed by @ntBre on 2025-08-08 21:43_

---

_Review request for @carljm removed by @ntBre on 2025-08-08 21:43_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-08 21:43_

---

_Review requested from @BurntSushi by @ntBre on 2025-08-08 21:43_

---

_@MichaReiser approved on 2025-08-10 13:00_

---

_Comment by @MichaReiser on 2025-08-10 13:00_

Is this marked as internal because the change hasn't been released yet?

---

_Comment by @ntBre on 2025-08-10 13:36_

> Is this marked as internal because the change hasn't been released yet?

Yep, that was my thinking. Otherwise it's a bug.

---

_@BurntSushi approved on 2025-08-11 11:21_

---

_Merged by @ntBre on 2025-08-11 12:52_

---

_Closed by @ntBre on 2025-08-11 12:52_

---

_Branch deleted on 2025-08-11 12:52_

---
